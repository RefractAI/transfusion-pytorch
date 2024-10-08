<img src="./transfusion.png" width="400px"></img>

## Transfusion - Pytorch (wip)

Pytorch implementation of [Transfusion](https://www.arxiv.org/abs/2408.11039), "Predict the Next Token and Diffuse Images with One Multi-Modal Model", from MetaAI.

Once completed, will also extend this to flow matching, as well as audio, video, perhaps even policies.

## Install

```bash
$ pip install transfusion-pytorch
```

## Usage

One modality, say images

```python
from torch import randint, randn
from transfusion_pytorch import Transfusion

model = Transfusion(
    num_text_tokens = 256,
    dim_latent = 384,
    transformer = dict(
        dim = 512,
        depth = 8
    )
)

text_and_images = [
    [randint(0, 256, (16,)), randn(4, 384), randint(0, 256, (8,)), randn(6, 384)],
    [randint(0, 256, (16,)), randn(7, 384), randint(0, 256, (5,)), randn(2, 384), randint(0, 256, (9,))]
]

loss = model(text_and_images)

loss.backward()
```

Multiple different modalities

```python
from torch import randint, randn
from transfusion_pytorch import Transfusion

model = Transfusion(
    num_text_tokens = 256,
    dim_latent = (384, 192), # specify multiple latent dimensions
    transformer = dict(
        dim = 512,
        depth = 8
    )
)

# then for the Tensors of type float, you can pass a tuple[int, Tensor] and specify the modality index in the first position

text_images_and_audio = [
    [randint(0, 256, (16,)), (0, randn(4, 384)), randint(0, 256, (8,)), (1, randn(6, 192))],
    [randint(0, 256, (16,)), randn(7, 384), randint(0, 256, (5,)), (1, randn(2, 192)), randint(0, 256, (9,))]
]

loss = model(text_images_and_audio)

loss.backward()
```

## Citations

```bibtex
@inproceedings{Zhou2024TransfusionPT,
    title  = {Transfusion: Predict the Next Token and Diffuse Images with One Multi-Modal Model},
    author = {Chunting Zhou and Lili Yu and Arun Babu and Kushal Tirumala and Michihiro Yasunaga and Leonid Shamis and Jacob Kahn and Xuezhe Ma and Luke Zettlemoyer and Omer Levy},
    year   = {2024},
    url    = {https://api.semanticscholar.org/CorpusID:271909855}
}
```

```bibtex
@misc{Rubin2024,
    author  = {Ohad Rubin},
    url     = {https://medium.com/@ohadrubin/exploring-weight-decay-in-layer-normalization-challenges-and-a-reparameterization-solution-ad4d12c24950}
}
```

```bibtex
@article{Nguyen2024MinPS,
    title   = {Min P Sampling: Balancing Creativity and Coherence at High Temperature},
    author  = {Minh Nguyen and Andrew Baker and Andreas Kirsch and Clement Neo},
    journal = {ArXiv},
    year    = {2024},
    volume  = {abs/2407.01082},
    url     = {https://api.semanticscholar.org/CorpusID:270870613}
}
```
