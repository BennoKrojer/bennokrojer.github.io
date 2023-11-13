# Elevator Pitch & ELI5 of my first PhD project: Image Retrieval from Contextual Descriptions

Created: May 30, 2022 9:26 AM
Tags: Conferences, Grounding, NLP, Research

I recently presented my first paper at a conference in person: [Image Retrieval from Contextual Descriptions](https://aclanthology.org/2022.acl-long.241/).

 ACL 2022 in Dublin was an amazing week but I noticed that I did not have a **super intuitive and structured elevator pitch or ELI5 prepared**.

So here it is, on different levels!

### 2-sentence-pitch

ImageCoDe is a new challenging benchmark for contextual vision-and-language understanding. The task is to select the right image from 10 minimally contrastive images based on a complex description and models do terribly!
**highlights phenomena in example(s)** (more on our [interactive demo](https://huggingface.co/spaces/BennoKrojer/imagecode-demo)!)

![Untitled](Elevator%20Pitch%20&%20ELI5%20of%20my%20first%20PhD%20project%20Imag%20ca8300450f5140849e2a5475354f2e94/Untitled.png)

### 3-minute-pitch

ImageCoDe is a new challenging **benchmark for contextual vision-and-language understanding**. The task is to select the right image from 10 minimally contrastive images based on a complex description and models do terribly!

**highlights phenomena example(s)** (more on our [interactive demo](https://huggingface.co/spaces/BennoKrojer/imagecode-demo)!)

We **crowdsource 21k descriptions** on more than 90k images (so **9k sets of minimally contrastive images**).
First, we get super similar images from video frames and also some from static images (but videos are way more interesting).
Second, we give people 10 such images and ask them to distinguish a randomly selected one from the others (with instructions like “no mentions of other images explicitly”).

Third, we give such a description with 10 images to retriever worker(s). Only if they retrieve it correctly, it stays in our data!

We **evaluate CLIP, ViLBERT and UNITER in 5 different settings**, and I will highlight three here:

1. zero-shot: all around ~20% accuracy (10% is random chance of picking the right image)
2. fine-tuning (maximize/minimize the text-image matching-score) with hard negatives in the same batch: improves to 24-28%
3. add **ContextualTransformer with temporal embeddings (for videos) on top of e.g. CLIP** output  vector that allows model to compare all images in one forward pass: small improvements with our **strongest model based on CLIP with ~30% Accuracy**

Our best model is far from human performance of 91%!

**Now the question everyone should have: Why do we need yet another vision-and-language benchmark? What’s new?**

My answer:

It is a mix of several factors that make this a new unique challenge that is not trivial for models:

- we annotate a long list of phenomena that occur frequently in the data, some of which other tasks cover less:
    - **contextuality** (assessing images in isolation is not enough) & **pragmatics** (semantic truth might be misleading)
    (This happens since descriptions only contain relevant differences while also being stand-alone captions)
    - temporality, negation, occlussion/visibility (things are out of frame or covered), nuances (referring to very small/non-salient details),…
- choosing **10 as number of images leads to more complex and multi-hop reasoning** since more factors have to be considered
- this above also means **descriptions are often long (several sentences) and complex** since one often needs more facts to distinguish from 9 others than just 1 other image: “her mouth is slightly open” can be enough for 1 image
- **minimal contrastiveness** of images should test compositional representations and is hopefully less prone to spurios correlations (see Contrast Sets)
- Since the images are so similar, the salient aspects are often the same across frames and only non-salient aspects change. **Focusing and zooming in on non-salient aspects** (while also keeping salient ones in mind) is a new challenge for models!
- high similarity and large number of images of images means that in practice humans have to use **slower “System 2” reasoning to accomplish the task** most of the time
- we include **diverse domains** (3 different video datasets, and also Open Images)

If you like this, consider submitting to our [leaderboard](https://mcgill-nlp.github.io/imagecode/)!

### ELI5

I think an Explain-to-me-like-I’m-5 type of explanation makes most sense when you know someone’s background and familarity and what they are curious about. So I would ask that first.

However I would probably **generally speak about how in vision-and-language research or more broadly in language grounding you have to come up with good tasks** that measure phenomena you care about. I would tell them about a few examples in the past and how ImageCoDe fits into this.

I would **describe what kind of skills a model must have to solve such a task as ImageCoDe**:

- connect both modalities (grounding language to parts of the image)
- handle complexities in language such as negation, pragmatics, commonsense or multiple facts
- handle complexities in vision such as zooming arbitrarily deep into details, occlusion, temporal dynamics
- compare images in a single forward pass

It would also be enlightening to some curious outsider to **learn what happens “behind the scenes” when a dataset is created**: the crowdsourcing, how to find workers and write instructions, make the data accessible to the public via tools like Huggingface Datasets or an interactive demo