# Building an LLM from Scratch
learning how to build a Large Language Model in Python, from the ground up 
no APIs, just writing the actual math and architecture myself so I understand 
what's happening under the hood. Closely following Sebastian Raschka's book
"Build a Large Language Model from Scratch." 
**Tokenization (where I'm at)**   
- first i started with learning how to download raw training text from the internet into the code 
  (a short story, used as a sample dataset)
- then i Understood what tokens are and how actual text becomes tokens, then 
  how they becomes token IDs
- Also understood how tokenizers work under the hood by reading Sebastian's 
  explanation of it
- Split the text into words/punctuation using regex
- Built a vocabulary 4,690 tokens down to 1,130 unique IDs
- wrote and understood how an encoder works by turning text into tokenid
- and decoder which turns tokenid back to text tokens and tested them
- My tokenizer breaks on words that are out of its vocabulary.
- Started using tiktoken GPT-2's byte pair encoding tokenizer
- understood how it works by breaking unknown words into smaller known pieces instead of crashing.

