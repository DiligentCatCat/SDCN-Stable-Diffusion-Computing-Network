<p align="center"><a href="https://sdcn.info" target="_blank" rel="noopener noreferrer"><img width="300" src="https://www.sdcn.info/static/media/logo.5c5b8351d8cee5d2cb97d5b6e3176425.svg" alt="SDCN logo"></a></p>

<p align="center">
  <a href="https://github.com/fiatrete/SDCN-Stable-Diffusion-Computing-Network/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License"></a>
  <a><img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs Welcome"></a>
  <a href="https://github.com/fiatrete/SDCN-Stable-Diffusion-Computing-Network/graphs/contributors"><img src="https://img.shields.io/github/contributors/fiatrete/SDCN-Stable-Diffusion-Computing-Network" alt="GitHub Contributors" /></a>
  <a href="https://github.com/fiatrete/SDCN-Stable-Diffusion-Computing-Network/commits/main"><img src="https://img.shields.io/github/last-commit/fiatrete/SDCN-Stable-Diffusion-Computing-Network" alt="Last Commit"></a>
</p>

<p align="center">
  <a href="https://www.sdcn.info" target="_blank">Website</a> |
  <a href="https://www.sdcn.info/play" target="_blank">Playground</a> |
  <a href="doc/api.md">API Docs</a>
</p>


## Overview

**[SDCN](https://www.sdcn.info) is an infrastructure for sharing Stable Diffusion computing power**

- **Decentralization**: By running the SDCN node program, users can register their idle computing resources with the SDCN network
- **Trustlessness**: SDCN abstracts the capabilities of Stable Diffusion into a set of atomic interface calls and hides the computing process from application developers
- **Powerful**: Application developers can quickly develop their own applications based on the Stable Diffusion related capabilities provided by SDCN, without worrying about how these interfaces are implemented or how computing power is provided

![SDCN structure](imgs/sdcn_structure_image.png)

- SDCN Node
  - Executes image generation tasks
  - Use Stable Diffusion WebUI with API mode directly
- SDCN Server
  - Manage and route image generation tasks to SDCN Nodes
  - Hide the image generating details and expose a standard interface to application developers
  - Implement SDCN server with openresty
- API
  - txt2img
  - img2img
  - integrate
  - more APIs under developing

**Why SDCN?**

- Everyone should have the ability to use AI freely, AI will be a public good
- For the public, the cost of trying various ways of stable diffusion is too high
    - It's difficult to set up a stable diffusion runtime environment on your own
    - Computer performance may not support it
    - Downloading code from GitHub and running Stable Diffusion WebUI is beyond the ability of most non-programmers(even programmers)
    - Learning about models and prompt knowledge requires high learning costs
- More application developers should be supported in popularizing AI capabilities to the public
- For application developers, the cost of building a publicly available image generation service is too high
    - Application developers should focus on implementing business requirements
- The utilization of GPU computing power for home and cloud procurement is very low, leading to significant wastage

</br>

## Getting Started

Try out SDCN functionalities in [SDCN website](https://www.sdcn.info/).

🎈Feel free to file tickets for bugs or feature requests. 

</br>

## 📱 How-to: try out SDCN API

SDCN provide SaaS-like API from [https://api.sdcn.info](https://api.sdcn.info)


### Using `sdcn_run.py` script

Try the sample code in `example` folder. 

You can modify the `example/params-xxx.json` to experiment with different parameter combinations.

```bash
python3 sdcn_run.py txt2img params-txt2img.json OUTPUT_IMAGE.png
python3 sdcn_run.py img2img params-img2img.json ORIGINAL_IMAGE.png OUTPUT_IMAGE.png
```

### Using `curl`
1. Install `curl` and `jq`
```bash
brew install curl jq
```
2. Enter `example` folder and execute
```bash
cd example
cat params-txt2img.json \
| curl --location --request POST 'https://api.sdcn.info/txt2img' \
--header 'Content-Type: application/json' -d @- \
| jq '.images[0]' |tr -d '\"' | tr -d '\\' | base64 -d > out.png
```

👉 **SDCN API** refer to [API Docs](doc/api.md)

</br>



## 🌐 How-to: contribute computing power to sdcn.info


1. Register a `donor account` on [sdcn.info](https://www.sdcn.info/) 
2. Install lastest [Stable Diffusion WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) on your PC or Server
3. Install the following models & loras::

- `chillout_mix`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/chilloutmix_NiPrunedFp32Fix.safetensors)
- `clarity`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/clarity.safetensors)
- `anything-v4.5-pruned`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/anything-v4.5-pruned.safetensors)
- `koreanDollLikeness_v10`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/koreandolllikeness_V10.safetensors)
- `stLouisLuxuriousWheels_v1`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/stLouisLuxuriousWheels_v1.safetensors)
- `taiwanDollLikeness_v10`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/taiwanDollLikeness_v10.safetensors)
- `kobeni_v10`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/kobeni_v10.safetensors)
- `thickerLinesAnimeStyle_loraVersion`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/thickerLinesAnimeStyle_loraVersion.safetensors)

4. Startup Stable Diffusion WebUI with `--listen --api --share` argument 

```bash
bash webui.sh --listen --api --share
```
> *You will get a public URL like `https://f00bfa54-7b3c-476b.gradio.live`*

5. Login `donor account` on [sdcn.info](https://sdcn.info) and register the public URL to `global node list`



<br>

## 🔨 How-to: run sdcn-server locally in Docker

1. Install lastest [Stable Diffusion WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) and [Docker](https://github.com/jenkinsci/docker) on your PC or Server

2. Install the following models & loras:

- `chillout_mix`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/chilloutmix_NiPrunedFp32Fix.safetensors)
- `clarity`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/clarity.safetensors)
- `anything-v4.5-pruned`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/anything-v4.5-pruned.safetensors)
- `koreanDollLikeness_v10`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/koreandolllikeness_V10.safetensors)
- `stLouisLuxuriousWheels_v1`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/stLouisLuxuriousWheels_v1.safetensors)
- `taiwanDollLikeness_v10`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/taiwanDollLikeness_v10.safetensors)
- `kobeni_v10`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/kobeni_v10.safetensors)
- `thickerLinesAnimeStyle_loraVersion`, [download](https://huggingface.co/fiatrete/sdcn-used-models/resolve/main/thickerLinesAnimeStyle_loraVersion.safetensors)

3. Startup Stable Diffusion WebUI with `--listen --api` argument 

```bash
bash webui.sh --listen --api
```

4. Start sdcn-server locally in docker with [Docker Compose](https://github.com/docker/compose):
```
docker-compose up -d 
```

>*Now your sdcn-server is available on "[http://127.0.0.1:6006](http://127.0.0.1:6006/)"*

5. Register your Stable Diffusion WebUI instance as a SDCN node:

```bash
curl -XPOST 'https://api.sdcn.info/admin/regworker' -d '{"worker":"http://yourlocalip:7860","owner":"yourname","nodeId":"yournodeid"}'
```



> ⚠️ *Please note that you must use the local IP address! You cannot use 127.0.0.1 or 'localhost' since our docker container's `hostnet` is not enabled.* </br></br> 👉 *You can unregister an instance if you need:*
>```bash
>curl -XPOST 'https://api.sdcn.info/admin/unregworker' -d '{"worker":"http://yourlocalip:7860"}'
>```


6. Config SERVICE_PREFIX in `example/sdcn_run.py` to "[http://127.0.0.1:6006](http://127.0.0.1:6006/)". 

```python
SERVICE_PREFIX = 'http://127.0.0.1:6006'
```

7. Execute the example with your local sdcn-server:

```bash
python3 sdcn_run.py txt2img params-txt2img.json OUTPUT_IMAGE.png
```

</br>


## Roadmap

- [x] Management GUI for computing power donors
    - [x] Add login to the SDCN website
    - [ ] CRUD management of computing power provided by donors
- [ ] Workload ranking page
    - [ ] Node-based workload ranking
    - [ ] Donor-based workload ranking
    - [ ] Model-based workload ranking
    - [ ] API-based workload ranking
- [ ] Basic SDCN functional interfaces
    - [ ] Scale interface
    - [ ] Inpaint interface
    - [ ] Support for controlnet
- [ ] Enrich examples in the playground
    - [ ] 2D to 3D style conversion
    - [ ] 3D to 2D style conversion
- [ ] One-click installation of Stable Diffusion WebUI as an SDCN node (Windows & Linux supported)
    - [ ] Customize Stable Diffusion WebUI installer
    - [ ] Automatically download necessary model files
    - [ ] Customize SDCN node daemon as intermediary for communication between Stable Diffusion WebUI and SDCN server
- [ ] Plugin mechanism for the playground
    - [ ] A pipeline-based workflow editor running in web form
    - [ ] A UI generator that generates interactive web pages based on input/output parameters
    - [ ] Plugin code sharing mechanism? (Need to discuss)
- [ ] Image Stream (user generated by the SDCN tool and share to sdcn.info)
    - [ ] Sharing mechanisms for images generated through API or playground
    - [ ] A page to display shared images in a waterfall flow
- [ ] Constraints for the use of computing resources
    - [ ] APP-Key mechanism
- [ ] Task scheduling mechanism
    - [ ] Schedule to nodes with required models already loaded
    - [ ] Global load balancing


</br>

## License

This project is licensed under the [MIT license]

[MIT license]: https://github.com/fiatrete/SDCN-Stable-Diffusion-Computing-Network/blob/main/LICENSE

</br>

## Credits

- Stable Diffusion - https://github.com/CompVis/stable-diffusion
- Stable Diffusion web UI - https://github.com/AUTOMATIC1111/stable-diffusion-webui
- Ant Design - https://github.com/ant-design/ant-design
