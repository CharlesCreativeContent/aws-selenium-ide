# Selenium IDE and AWS Cloud Development: <a target="_blank" href="https://www.canva.com/design/DAGEl541AD8/y7s5u40Upi8Wbxr8lijHBA/view?utm_content=DAGEl541AD8&utm_campaign=designshare&utm_medium=link&utm_source=editor">Slides Here</a>
Scripts to automate the deployment of cloud infrastructure using the browser automation tool selenium-ide

<img src="https://github.com/CharlesCreativeContent/myImages/blob/main/images/Browser%20Automation%20&%20AWS%20Cloud%20Development.jpg?raw=true" width="100%" alt="Slideshow Picture"/>

## Prerequisites

- You have installed Python 3.9+ and `pip`. See the [Python downloads page](https://www.python.org/downloads/) to learn more.
- You have a basic understanding of key concepts in BentoML, such as Services. We recommend you read [Quickstart](https://docs.bentoml.com/en/latest/get-started/quickstart.html) first.
- (Optional) We recommend you create a virtual environment for dependency isolation for this project. See the [Conda documentation](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) or the [Python documentation](https://docs.python.org/3/library/venv.html) for details.

## Install dependencies

```bash
git clone https://github.com/CharlesCreativeContent/BentoText2Video.git
cd BentoText2Video
pip install -r requirements.txt
```

## Run the BentoML Service

We have defined a BentoML Service in `service.py`. Run `bentoml serve` in your project directory to start the Service. You may also set the environment variable `COQUI_TTS_AGREED=1` to agree to the terms of Coqui TTS. We Currently have the lock_packages set to False in the bentofile.yaml, which bypasses the requirement of local builds. 

```python
$ COQUI_TOS_AGREED=1 bentoml serve .

2024-01-18T11:13:54+0800 [INFO] [cli] Starting production HTTP BentoServer from "service:XTTS" listening on http://localhost:3000 (Press CTRL+C to quit)
/workspace/codes/examples/xtts/venv/lib/python3.10/site-packages/TTS/api.py:70: UserWarning: `gpu` will be deprecated. Please use `tts.to(device)` instead.
  warnings.warn("`gpu` will be deprecated. Please use `tts.to(device)` instead.")
 > tts_models/multilingual/multi-dataset/xtts_v2 is already downloaded.
 > Using model: xtts
```

The server is now active at [http://localhost:3000](http://localhost:3000/). You can interact with it using the Swagger UI or in other different ways.

CURL

```bash
curl -X 'POST' \
  'http://localhost:3000/synthesize' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
  "text": "It took me quite a long time to develop a voice and now that I have it I am not going to be silent.",
  "lang": "en"
}' -o output.mp4
```

## Deploy to production

After the Service is ready, you can deploy the application to BentoCloud for better management and scalability. A YAML configuration file (`bentofile.yaml`) is used to define the build options and package your application into a Bento. See [Bento build options](https://docs.bentoml.com/en/latest/concepts/bento.html#bento-build-options) to learn more.

Make sure you have [logged in to BentoCloud](https://docs.bentoml.com/en/latest/bentocloud/how-tos/manage-access-token.html), then run the following command in your project directory to deploy the application to BentoCloud.

```bash
bentoml deploy .
```

Once the application is up and running on BentoCloud, you can access it via the exposed URL.

**Note**: Alternatively, you can use BentoML to generate a [Docker image](https://docs.bentoml.com/en/latest/guides/containerization.html) for a custom deployment.
