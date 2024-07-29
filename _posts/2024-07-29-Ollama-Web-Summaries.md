---
layout: post
title: Summarizing Web Pages with Ollama
published: true
tags: github tutorial
---

# Summarizing Web Pages with Ollama 
Hey everyone like all of you (hopefully), I too have been looking at large langauge models and trying to integrate them into my workflows in new and creative ways. In particular I've been enjoying working with the [Ollama](https://ollama.com/) project which is a framework for working with locally available open source large language models, aka do chatgpt at home for free. Here's a short script I created from Ollama's examples that takes in a url and produces a summary of the contents. I use this along with my read it later apps to create short summary documents to store in my obsidian vault. Thereby integrating pieces of information into my boarder and searchable second brain just by providing a url, eventually I want to hook this up to the newsletters I read so I can automate the process further. Here's the full script with added comments. 

```python

import argparse
import os
import datetime
from langchain_community.llms import Ollama
from langchain_community.document_loaders import WebBaseLoader
from langchain.chains.summarize import load_summarize_chain
from dotenv import load_dotenv

# set OLLAMA_MODEL env var or create a .env file with OLLAMA_MODEL set to the model of your choice
load_dotenv()

ollama_model = os.getenv("OLLAMA_MODEL","llama3")

def save_to_markdown(title, content, url, filename):
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    word_count = len(content["output_text"].split())
    with open(filename, "w", encoding="utf-8") as f:
        f.write(f"# {title}\n\n")
        f.write(f"**Source URL:** {url}\n\n")
        f.write(f"**Timestamp:** {timestamp}\n\n")
        f.write(f"**Word Count:** {word_count}\n\n")
        f.write(f"---\n\n")
        f.write(content["output_text"])

def main():
    # setting up commandline arguments
    parser = argparse.ArgumentParser(description="Summarize a webpage via a llm model available via ollama")
    parser.add_argument("website", type=str, help="The URL of the website to summarize.")
    parser.add_argument("-o", "--output", type=str, help="Output markdown file to save the summary. If not provided, output will be printed to stdout.")
    args = parser.parse_args()

    # load into langchain
    loader = WebBaseLoader(args.website)
    docs = loader.load()

    # invoke langchain 
    llm = Ollama(model=ollama_model)
    chain = load_summarize_chain(llm, chain_type="stuff")

    result = chain.invoke(docs)

    # Extract webpage title and other metadata
    title = "Webpage Summary"  # Default title if none is found
    if docs and docs[0].metadata and "title" in docs[0].metadata:
        title = docs[0].metadata["title"]

    if args.output:
        save_to_markdown(title, result, args.website,
                        args.output)
    else:
        word_count = len(result["output_text"].split())
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        print(f"# {title}\n")
        print(f"**Source URL:** {args.website}\n")
        print(f"**Timestamp:** {timestamp}\n")
        print(f"**Word Count:** {word_count}\n")
        print(f"---\n")
        print(result)

if __name__ == "__main__":
    main()

```

The peanut butter and jelly explanation of this:
1. take in webpage url as an argument
2. optionally take in a title for the summary to dump to (otherwise dump to stdout which you can pipe to elsewhere for example)
3. load .env file with selection of [model supported by Ollama](https://ollama.com/library)
4. load the webpage from the url and pull the webpage's text into a format that langchain can use. 
5. We run the summarize chain from langchain and use our ollama model as the large language model to generate our text. 
6. We parse the output, saving it or dumping it into stdout with helpful metadata. 