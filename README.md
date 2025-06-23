# CBIR-using-Hybrid-Encryption-Techniques

This project implements a secure, privacy-preserving image retrieval system using a **hybrid encryption** approach. It leverages **AES** for encrypting image data and **CKKS Homomorphic Encryption** for encrypted feature matching. Deep learning (ResNet-50) is used for extracting image features.

---

## Project Overview

- **Objective**: Enable encrypted image storage and retrieval without revealing raw image data or features.
- **Encryption**: 
  - **AES (CBC Mode)** for image encryption.
  - **CKKS Homomorphic Encryption** (via [TenSEAL](https://github.com/OpenMined/TenSEAL)) for secure feature encryption.
- **Model**: Uses a pre-trained **ResNet-50** model for extracting deep feature vectors.

---

## Project Structure

- `/data/`
  - `encrypted_images/`: Stores AES-encrypted image files.
  - `encrypted_features/`: Stores CKKS-encrypted feature vectors.
  - `decrypted_images/`: Stores output images after decryption during retrieval.
  - `mapping.json`: Maps encrypted features to their corresponding AES-encrypted images.
  - `save_path_public.pkl`: CKKS public context for encryption.
  - `save_path_secret.pkl`: CKKS secret context for decryption and retrieval.

---

## Part 1: Creating the Encrypted Image Database

1. The AES key is defined for encrypting the images.

2. The system ensures a JSON file exists to store the mapping between encrypted features and encrypted images.

3. The user uploads all images they want to store securely in the system.

4. The CKKS public context (`save_path_public.pkl`) is loaded.

5. A ResNet-50 model is loaded and modified to remove its final classification layer, so it can be used for feature extraction.

6. Each uploaded image is preprocessed (resized, normalized, and converted to tensor format) for compatibility with ResNet.

7. A feature vector is extracted from the image using the ResNet model.

8. The extracted feature vector is encrypted using CKKS homomorphic encryption and saved in the `encrypted_features` folder.

9. The original image is encrypted using AES and saved in the `encrypted_images` folder, retaining the original file extension.

10. The system updates a JSON mapping file, linking the encrypted feature file path to its corresponding encrypted image path.

11. This process repeats for all uploaded images, completing secure storage.

---

## Part 2: Retrieving Similar Images

Part 2: Retrieving Similar Images Based on a Query Image. Process is as follows:

1. The CKKS private context (used for decrypting feature vectors) is loaded (`save_path_secret.pkl`).

2. ResNet-50 model is loaded and modified by removing its final classification layer to use it for feature extraction.

3. The user uploads the query image for which visually similar matches are to be found.

4. The query image is preprocessed (resized, normalized, and converted to tensor format) for compatibility with ResNet.

5. A feature vector is extracted from the query image using the ResNet model and is then encrypted using CKKS homomorphic encryption.

6. The system loads all previously stored encrypted feature vectors from the `encrypted_features` folder.

7. For each stored encrypted feature vector, both the query and stored vectors are compared using cosine similarity.

8. If the similarity score exceeds 80%, the encrypted vector is considered a match.

9. The system uses the JSON file `mapping.json` to locate the corresponding AES-encrypted image for each matching vector.

10. The matched AES-encrypted images are decrypted using the AES key and displayed to the user.

11. The decrypted images are also saved to the `decrypted_images` folder, from where the user can download them.

---

## Technologies Used:

- Python 
- PyTorch & TorchVision 
- TenSEAL (Homomorphic Encryption)
- AES (from PyCryptodome) 
- NumPy, PIL, JSON, and OS utilities

---

## Example Output

- Query Image → Encrypted → Feature Extracted → Encrypted Feature
- Matching Encrypted Features → Cosine Similarity > 80% → Mapped → Decrypted AES Images → Displayed/Saved

---

