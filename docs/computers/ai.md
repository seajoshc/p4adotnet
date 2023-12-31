# AI/ML

- [Andrew Ng ML course](https://www.deeplearning.ai/courses/machine-learning-specialization/)
- [IBM Data Engineer certification](https://www.coursera.org/professional-certificates/ibm-data-engineer?utm_source%253DIBM%2526utm_medium%253Dinstitutions%2526utm_campaign%253DIBMBadge)
- [MSFT Gen AI for Beginners](https://microsoft.github.io/generative-ai-for-beginners/#%252F)
- [Hugging Face learn NLP course](https://huggingface.co/learn/nlp-course/chapter1/1)
- [AI for Everyone course](https://www.deeplearning.ai/courses/ai-for-everyone/)
- [Practical Deep Learning for Coders](https://course.fast.ai/) and [the Jupyter notebook based book](https://nbviewer.org/github/fastai/fastbook/tree/master)
- [Huggingface](https://huggingface.co/course/chapter1/1)
- [FastAPI Docker example](https://www.docker.com/blog/build-machine-learning-apps-with-hugging-faces-docker-spaces/)
- https://karpathy.ai/zero-to-hero.html

## General Notes

Model is a neural network function.

Models alone aren't good enough, they need to be trained and importantly you need the "weights" in order to have a ready to use model like the resnet18 model that "comes with" FastAI.

Fine tuning is taking a trained model and training it further on your data.

Learner is model + training data

Weights are parameters

- It used to be Input > Program > Output
- Now its Input + Weights > Model > Output
- With a training loop is Input + Weights + "Loss" from previous training > Model > Output + Loss (now do it all again N times to solve any computable function!)

## Embeddings

Credit and sources:

- [Simon Willison's embeddings article](https://simonwillison.net/2023/Oct/23/embeddings/)

Using an embeddings model, take content and turn it into a multidimensional array of floating point numbers. The length of the array is always the same regardless of the size of the content; the size is determined by the model used to create the embeddings.

Combine embeddings, or rather collections of, with a database and now you basically have semantic search.

To use Retrieval Augmented Generation (RAG), you take an LLM plus your collections of embeddings and combined. When a request comes in, first "semantic search" for related content within the embeddings then you shove that content (paragraph, sentences, whatever) into the prompt so the LLM will use that when generating the response. Wow genius...
