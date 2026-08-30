# Building an LLM from Scratch
learning how to build a Large Language Model in Python, from the ground up 
no APIs, just writing the actual math and architecture myself so I understand 
what's happening under the hood. Closely following Sebastian Raschka's book
"Build a Large Language Model from Scratch." 
  
  ## Tokenization  
- first i started with learning how to download raw training text from the internet into the code 
  (a short story, used as a sample dataset)
- then i Understood what tokens are and how actual text becomes tokens, then 
  how they becomes token IDs
- Also understood how tokenizers work under the hood by reading Sebastian's 
  explanation of it
- Split the text into words/punctuation using regex
- the text had 4,690 total tokens, which mapped down to a vocabulary of 1,130 unique token IDs
- wrote and understood how an encoder works by turning text into tokenid
- and decoder which turns tokenid back to text tokens and tested them
- My tokenizer breaks on words that are out of its vocabulary.
- Started using tiktoken GPT-2's byte pair encoding tokenizer
- understood how it works by breaking unknown words into smaller known pieces instead of crashing.
- 
- ## Data sampling and batching
* learned that there is a limit to the amount of tokens that can be put into the model beacuse the model expects fixed size input
* you have to chop it into fixed size chunks and thats where i used pytorch for the first time
* used torch.tensor() to convert my token ID lists into PyTorch tensors which is a different type of list since plain list cant be run through the model math
* the target for each chunk is just the same chunk shifted forward by 1 token thats how it learns to predict the next word
* built GPTDatasetV1 as a pytorch Dataset so it can plug into pytorch's training tools
* max_length controls how big each window is, and stride controls how far the window's starting point moves before grabbing the next one.
* small stride = more overlap = more chunks = more memory/time, but a lot of it is repeated info
* big stride (stride = max_length) = no overlap, each token used about once
* wrote create_dataloader_v1 which uses pytorch's DataLoader to handle batching and shuffling for me
* batching is how many windows (chunks) get grouped together and fed to the model in one go
* tested it with small numbers (max_length=4, stride=1 vs stride=4) just to see the shapes and understand it before using real values
  ## Creating token embeddings
 * learned that token IDs are just random labels the model cant do 
  real math with them since the numbers dont mean anything (id 133 isnt 
  "less" than id 5000)
 * so each token id gets turned into a vector of numbers instead called an 
  embedding this is something the model can actually learn from
 * built an embedding layer with torch.nn.Embedding(vocab_size, output_dim), 
  its basically just a lookup table
 * vocab_size Total number of unique words in our dictionary this dictates the number of rows
 * output_dim The amount of numbers used to represent one single word it dictates the number of columns
 * the table starts with random numbers but manual_seed keeps those numbers exactly the same for every test run.
 * calling the embedding layer on a token id just grabs that row, no real 
  math just a lookup
 * did the same for the list of tokenids it only grabs the rows matching those ids and lines them up in the same order the ids appear in the list
 * learned that during training the model constantly updates the numbers for each word the ones that show up in similar sentences end up with similar numbers that's how it learns which words belong together
 ## Encoding word positions
 * if the same word show up twice in a sentence they get the exact same vector so the model dosent know order and order completely changes meaning of a word
 * the fix is positional embeddings we create a separate embedding matrix that looks up by position (0, 1, 2, 3...)
 * now we add the token embedding and the position embedding and the input embedding will look different depending on where the word is located
 * token_embedding + positional_embedding = input_embedding
 * now even if the same word appears twice it ends up with different final vectors because the position part is different
