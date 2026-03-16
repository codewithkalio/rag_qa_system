# Observations

## Documentation of Failures
1. Initial testing was done on a long, wordy book.  This was too challenging to fact check.  Switched to an easier document.  

2.  First chunk of the PDF contains website header and footer information (PDF was generated from a website.)
Fix:  added 'chunks = chunks[1:]  # Skip the first chunk (nav/header noise)' after splitting.  

3.  Testing using concepts that are found throughout the document made it difficult to verify.  
Fix: Switched to concepts with fewer instances in the article.  

## Chunk Size Decision
Chunk sizes of 500 and 800 were tested.  There was little difference in the outcome.  Chunk size 800 was chosen since it helped contain the header/footer on the first page so it could be excluded.  

## Adjusting System Prompt to Specifically Answer Questions Using the Reference Material
Initial prompt was too generic resulting in the model embellishing with accurate but tangental information that did not directly answer the question.

system_prompt = (
    "You are a helpful assistant that answers questions using a document retrieval tool."
    "Always call the retrieval tool before answering — never rely on your own knowledge."
    "Base your answer only on the retrieved content."
    "Answer only what is directly asked. Do not include related but unrequested information."
    "If the retrieved content does not contain enough information to answer the question, say so clearly — do not guess."
    "Keep answers concise and specific. "
    "At the end of each answer, cite the source document and page number from the metadata."
)


# What Could Be Improved
- Add more documents
- Address other document formats
- Add a UI element
