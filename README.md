# 🐢 Tortoise TTS pay-as-you-go API

A "pay-as-you-go" API for [Tortoise TTS](https://github.com/neonbjb/tortoise-tts/). It uses [Modal](https://modal.com/) underneath.

Tortoise is one of the best text-to-speech systems ever built, but it currently requires the user to deploy their own service on a GPU which can be time-consuming, difficult & expensive. The alternative is to use paid services which offer a monthly pricing tier and are closed-off. This repository aims to provide a usage-based, pay-as-you-go API based on open-source code instead.

We have made some improvements to Tortoise to make the inference **~30% faster**, and welcome contributions on our [repo](https://github.com/metavoicexyz/tortoise-tts) to improve it further!

We also provide a Python API wrapper that can used for easy integration into your Python code.

## Usage
You can use the HTTP end-point directly, or the provided Python wrapper.

### HTTP end-point
The synthesis can be done by send a POST request to "https://vatsalaggarwal--tts-app.modal.run". 

#### Headers
Make sure to include headers that keep the connection alive sufficiently long as the inference can take 30s-150s.
```
"Connection: keep-alive",
"Keep-Alive: timeout=600, max=100",
```

#### Body
- `api_key`: Key that manages payment (note the charges are "at-cost"). It can be generated by visiting https://tts.themetavoice.xyz
- `text`: Text you want to synthesize. Normalized text (1 -> "one") with proper punctuation works best. 225 characters is probably the maximum you should try given how the model was trained, and the constraints on the Modal backend.
- `voices`: String specifying which voice should be used to synthesize the text. There are four ways to get different voices out of this model:
    - `"random"`: The model randomly picks a voice in its embedding space
    - `"<name>"`: Use one of the voices the model was trained on (e.g. `train_grace`)
        - Choices are: `angie`, `applejack`, `cond_latent_example`, `daniel`, `deniro`, `emma`, `freeman`, `geralt`, `halle`, `jlaw`, `lj`, `mol`, `myself`, `pat`, `pat2`, `rainbow`, `snakes`, `tim_reynolds`, `tom`, `train_atkins`, `train_daws`, `train_dotrice`, `train_dreams`, `train_empire`, `train_grace`, `train_kennard`, `train_lescault`, `train_mouse`, `weaver`, `william`
    - `"<name>&<name>"`: Combine two voices (e.g. `train_grace&emma`)
    - `""`: (Zero-shot) Used with `target_file` parameter described next to synthesize text in the voice of an utterance given by the user.
- [optional] `target_file`: A pointer to a mp3 or wav file from a speaker whose voice you're trying to synthesize text in. This should be uploaded to a static file store (like a public s3 bucket).
    - This parameter is unused if `voices` is specified, and shouldn't be specified as part of the body.
    - If this parameter is used, make sure that `voices` is set to `""`. 

### Python Wrapper
`run_api.py` provides an example.

## Contributing

### TODOs
- Use GPT-3 to perform text normalisation
- Inference optimisations
    - TODO: add ideas
- Finetuning code 

### Developer environment
- `python=3.10.8` (important for Modal)
