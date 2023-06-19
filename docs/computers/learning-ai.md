# Learning AI/ML

- [AI for Everyone course](https://www.deeplearning.ai/courses/ai-for-everyone/)
- [Practical Deep Learning for Coders](https://course.fast.ai/) and [the Jupyter notebook based book](https://nbviewer.org/github/fastai/fastbook/tree/master)
- [Huggingface](https://huggingface.co/course/chapter1/1)
- [FastAPI Docker example](https://www.docker.com/blog/build-machine-learning-apps-with-hugging-faces-docker-spaces/)
- https://karpathy.ai/zero-to-hero.html

## Notes

Model is a neural network function.

Models alone aren't good enough, they need to be trained and importantly you need the "weights" in order to have a ready to use model like the resnet18 model that "comes with" FastAI.

Fine tuning is taking a trained model and training it further on your data.

Learner is model + training data

Weights are parameters (hmm followup on this)

- It used to be Input > Program > Output
- Now its Input + Weights > Model > Output
- With a training loop is Input + Weights + "Loss" from previous training > Model > Output + Loss (now do it all again N times to solve any computable function!)
