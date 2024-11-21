# explore-eqa-devcontainer

Dev container &amp; Dockerfile for https://github.com/Stanford-ILIAD/explore-eqa.

## Usage

### Workspace setup

1. Clone target repository: `git clone https://github.com/Stanford-ILIAD/explore-eqa.git`.
2. Download and extract `hm3d-train-habitat-v0.2.tar` to `explore-eqa/data/hm3d-train-habitat-v0.2`, as documented [here](https://github.com/matterport/habitat-matterport-3dresearch#-downloading-hm3d-v02:~:text=32G-,hm3d%2Dtrain%2Dhabitat%2Dv0.2.tar,-train).
3. Put your HF token with access to the models at `explore-eqa/data/huggingface/token`.
4. Adjust `cfg/*` files as needed.
    - Set `scene_data_path` to `data/hm3d-train-habitat-v0.2`.
    - Set `hf_token` to your HF token (if present).

### Build and run

1. Open the repository in VS Code.
2. If you havn't already, install the Dev Containers extension (`ms-vscode-remote.remote-containers`).
3. Command palette (`Ctrl+Shift+P`): `Dev Containers: Reopen in Container`.

> If you can't tolerate the slow build speed, consider building it in background as described in [this post](https://pro-2684.github.io/?page=dev_containers#build-in-background).

### Post-setup

After you've connected to the container, run the following commands:

```bash
conda init --all
# Restart the shell
conda activate base && pip install -e . && cd prismatic-vlms && pip install -e . && cd ..
cd CLIP && pip install -e . && cd .. # For running CLIP-based exploration
```

Note that this step must be done every time you rebuild the container, but not every time you reopen it :)

### Running the testcases

Now you can run the testcases via:

```bash
python run_vlm_exp.py -cf cfg/vlm_exp.yaml
```

It could take quite a while to run the testcases, so you may want to run it in background:

```bash
nohup python run_vlm_exp.py -cf cfg/vlm_exp.yaml &
```

✨ Enjoy your day!
