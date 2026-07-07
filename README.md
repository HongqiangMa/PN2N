# PN2N

Poisson-thinning-enabled self-supervised denoising for photon-limited microscopy.
![PN2N example result](PN2N_example.png)

PN2N_Colab is a Colab-based implementation for self-supervised denoising of photon-limited microscopy images. It uses Poisson thinning to generate statistically independent training pairs from a single photon-counted image.

## Run in Google Colab

[![Open PN2N in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/HongqiangMa/PN2N/blob/main/PN2N_Colab.ipynb)

## Manuscript

Poisson thinning enables self-supervised denoising in photon-limited microscopy

## Author

Hongqiang Ma, PhD  
Department of Bioengineering  
University of Illinois Urbana-Champaign  
Beckman Institute for Advanced Science and Technology  
Email: mhq@illinois.edu

## Usage

1. Open the notebook in Google Colab.
2. Select `Runtime > Change runtime type`.
3. Choose a GPU runtime.
4. Run the notebook from top to bottom.
5. Upload your own microscopy image or use the provided demo image.
6. Set the camera gain and offset to convert raw images into photon units.
7. Train PN2N and run inference on the full photon-count image.

## Input data

Input images should be converted into photon units before Poisson thinning:

photon_image = np.maximum((raw_DN - camera_offset) / camera_gain, 0)

## Main user inputs

The main parameters are set in the final section of the Colab notebook: **User parameters and batch execution**.

| Parameter in notebook | Default value | Description |
|---|---:|---|
| `data_dir` | `/content/drive/MyDrive/ColabNotebooks/images` | Google Drive folder containing input `.tif` or `.tiff` images |
| `save_dir` | `/content/drive/MyDrive/ColabNotebooks/results` | Google Drive folder where PN2N outputs, model weights and logs are saved |
| `lr` | `1e-3` | Learning rate for AdamW optimization |
| `batch_size` | `32` | Number of random Poisson-thinned patches per training batch |
| `patch_size` | `256` | Size of randomly cropped 2D training patches |
| `epochs` | `1000` | Maximum number of training epochs |
| `seed` | `42` | Random seed for reproducible training and patch sampling |
| `camera_offset` | `0` | Camera black level or electronic offset. Use `0` if the data are already offset-corrected |
| `camera_gain` | `1` | Camera gain used for DN-to-photon conversion. Use `1` if the data are already in photon units |

The notebook processes each TIFF file in `data_dir` independently. For each image or stack, PN2N trains a separate U-Net model and saves the denoised output with the suffix `_PN2N`.

## Fixed internal settings

Some PN2N settings are fixed inside the notebook rather than exposed as user-input fields:

| Internal setting | Value | Description |
|---|---:|---|
| Poisson-thinning probability | `0.5` | Each photon count is binomially split into two thinned views with matched expected photon counts |
| Network | 2D U-Net | Compact encoder-decoder denoising network |
| Initial U-Net channels | `32` | Number of feature channels in the first encoder layer |
| U-Net depth | `4` | Number of encoder-decoder levels |
| Loss function | L1 loss | Used for bidirectional self-supervised training |
| Training strategy | Bidirectional | View 1 predicts view 2 and view 2 predicts view 1 |
| Padding | Reflection padding | Used during inference to reduce boundary artifacts |
| Output suffix | `_PN2N` | Added to the denoised TIFF filename |

## Notes

The notebook is designed for Google Colab. It was tested using a Colab GPU runtime. For a quick test, use fewer epochs. For manuscript-level restoration, use the full training setting described in the manuscript.

## Citation

If you use PN2N, please cite:

Ma, H. Poisson thinning enables self-supervised denoising in photon-limited microscopy.

## License

This code is released under the MIT License.

## Manuscript

Poisson thinning enables self-supervised denoising in photon-limited microscopy

## Author

Hongqiang Ma, PhD  
Department of Bioengineering  
University of Illinois Urbana-Champaign  
Beckman Institute for Advanced Science and Technology  
Email: mhq@illinois.edu

## Usage

1. Open the notebook in Google Colab.
2. Select `Runtime > Change runtime type`.
3. Choose a GPU runtime.
4. Run the notebook from top to bottom.
5. Upload your own microscopy image or use the provided demo image.
6. Set the camera gain and offset to convert raw images into photon units.
7. Train PN2N and run inference on the full photon-count image.

## Input data

Input images should be converted into photon units before Poisson thinning:

photon_image = np.maximum((raw_DN - camera_offset) / camera_gain, 0)

## Main user inputs

The main parameters are set in the final section of the Colab notebook: **User parameters and batch execution**.

| Parameter in notebook | Default value | Description |
|---|---:|---|
| `data_dir` | `/content/drive/MyDrive/ColabNotebooks/images` | Google Drive folder containing input `.tif` or `.tiff` images |
| `save_dir` | `/content/drive/MyDrive/ColabNotebooks/results` | Google Drive folder where PN2N outputs, model weights and logs are saved |
| `lr` | `1e-3` | Learning rate for AdamW optimization |
| `batch_size` | `32` | Number of random Poisson-thinned patches per training batch |
| `patch_size` | `256` | Size of randomly cropped 2D training patches |
| `epochs` | `1000` | Maximum number of training epochs |
| `seed` | `42` | Random seed for reproducible training and patch sampling |
| `camera_offset` | `0` | Camera black level or electronic offset. Use `0` if the data are already offset-corrected |
| `camera_gain` | `1` | Camera gain used for DN-to-photon conversion. Use `1` if the data are already in photon units |

The notebook processes each TIFF file in `data_dir` independently. For each image or stack, PN2N trains a separate U-Net model and saves the denoised output with the suffix `_PN2N`.

## Fixed internal settings

Some PN2N settings are fixed inside the notebook rather than exposed as user-input fields:

| Internal setting | Value | Description |
|---|---:|---|
| Poisson-thinning probability | `0.5` | Each photon count is binomially split into two thinned views with matched expected photon counts |
| Network | 2D U-Net | Compact encoder-decoder denoising network |
| Initial U-Net channels | `32` | Number of feature channels in the first encoder layer |
| U-Net depth | `4` | Number of encoder-decoder levels |
| Loss function | L1 loss | Used for bidirectional self-supervised training |
| Training strategy | Bidirectional | View 1 predicts view 2 and view 2 predicts view 1 |
| Padding | Reflection padding | Used during inference to reduce boundary artifacts |
| Output suffix | `_PN2N` | Added to the denoised TIFF filename |

## Notes

The notebook is designed for Google Colab. It was tested using a Colab GPU runtime. For a quick test, use fewer epochs. For manuscript-level restoration, use the full training setting described in the manuscript.

## Citation

If you use PN2N, please cite:

Ma, H. Poisson thinning enables self-supervised denoising in photon-limited microscopy.

## License

This code is released under the MIT License.
