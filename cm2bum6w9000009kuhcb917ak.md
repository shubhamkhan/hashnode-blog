---
title: "Securing Your JavaScript Applications by using AES Encryption and Decryption with CryptoJS"
seoTitle: "Securing Your JS Applications by Using AES Encryption and Decryption"
seoDescription: "The most widely used encryption technique is AES (Advanced Encryption Standard), which is both fast and secure."
datePublished: Wed Oct 16 2024 12:28:46 GMT+0000 (Coordinated Universal Time)
cuid: cm2bum6w9000009kuhcb917ak
slug: aes-encryption-decryption
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729074801257/135438f4-4509-4e07-bb7d-1862f205068e.webp
tags: decryption, javascript, security, encryption, typescript, aes, aes-encryption, ciphertext

---

In today's world, data security is a top priority for developers and businesses alike. One of the most widely used encryption techniques is **AES (Advanced Encryption Standard)**, which is both fast and secure. This article will walk you through how to implement AES encryption and decryption using JavaScript and the popular **CryptoJS** library.

By the end of this article, you will have a clear understanding of how to secure your data using AES encryption and decrypt it when needed.

### What is AES Encryption?

**AES** is a symmetric encryption algorithm, which means it uses the same secret key to encrypt and decrypt the data. AES is known for its strength and efficiency and is commonly used to protect sensitive information like passwords, credit card details, and personal information.

### The Need for Encryption in Modern Web Development

Whether you are building a personal blog, an e-commerce site, or a business app, you need to ensure that sensitive data is protected. **AES encryption** ensures that even if the data is intercepted, it remains unreadable without the secret key, keeping it secure from attackers.

### What is CryptoJS?

**CryptoJS** is a popular JavaScript library that makes it easy to implement various cryptographic algorithms, including AES encryption. You can use CryptoJS to perform encryption, hashing, and other cryptographic functions in your web or mobile applications.

### How AES Encryption and Decryption Works

Here’s a simple breakdown of the steps involved in encrypting and decrypting data using AES:

1. **Encryption:**
    
    * You take the plaintext data (in this case, a string).
        
    * A secret key is used to encrypt the data, making it unreadable.
        
    * The encrypted result is typically converted into a string format that can be easily stored or transmitted.
        
2. **Decryption:**
    
    * The encrypted string is taken and the secret key is used to decrypt it.
        
    * This reveals the original plaintext data.
        

### Let's Dive into the Code

Below are the functions for AES encryption and decryption using CryptoJS. These functions take a secret key stored in environment variables and a piece of text to either encrypt or decrypt.

#### AES Encryption Function

```typescript
const encryptWithSecretKey = (text: string): string | null => {
    try {
        const secretKey = process.env.NEXT_PUBLIC_SECRET_KEY!?.replace(/\\n/g, "\n");
        
        const iv = CryptoJS.lib.WordArray.random(16);
        const encrypted = CryptoJS.AES.encrypt(
          text,
          CryptoJS.enc.Hex.parse(secretKey),
          {
            iv: iv,
            padding: CryptoJS.pad.Pkcs7,
            mode: CryptoJS.mode.CBC,
          }
        );
        
        const encryptedBase64 = CryptoJS.enc.Base64.stringify(
          iv.concat(encrypted.ciphertext)
        );
        
        return encryptedBase64;
    } catch (error) {
        console.error("Decryption error:", error);
        return null;
  }
};
```

**Explanation:**

* We use the AES algorithm in **CBC (Cipher Block Chaining)** mode, which requires an initialization vector (**IV**) for added security.
    
* The `CryptoJS.AES.encrypt` method performs the encryption.
    
* The final encrypted output is Base64 encoded to make it URL-safe, replacing certain characters to ensure that it can be safely transmitted.
    

#### AES Decryption Function

```typescript
const decryptWithSecretKey = (encryptedText: string): string | null => {
  try {
    const fullCipher = CryptoJS.enc.Base64.parse(encryptedText);

    const iv = CryptoJS.lib.WordArray.create(fullCipher.words.slice(0, 4), 16);
    const ciphertext = CryptoJS.lib.WordArray.create(fullCipher.words.slice(4));

    const cipherParams = CryptoJS.lib.CipherParams.create({
      ciphertext: ciphertext,
    });

    const secretKey = process.env.NEXT_PUBLIC_SECRET_KEY!?.replace(
      /\\n/g,
      "\n"
    );

    const decrypted = CryptoJS.AES.decrypt(
      cipherParams,
      CryptoJS.enc.Hex.parse(secretKey),
      {
        iv: iv,
        padding: CryptoJS.pad.Pkcs7,
        mode: CryptoJS.mode.CBC,
      }
    );

    return decrypted.toString(CryptoJS.enc.Utf8);
  } catch (error) {
    console.error("Decryption error:", error);
    return null;
  }
};
```

**Explanation:**

* The decryption process starts by reversing the URL-safe Base64 transformation to get the original IV and ciphertext.
    
* The AES decryption is performed using the same secret key and IV that were used for encryption.
    
* If the decryption is successful, the original plaintext is returned; otherwise, an error is logged.
    

### Key Concepts in the Code

1. **Secret Key**: This is used to both encrypt and decrypt data. It's critical that this key remains secret, ideally stored in environment variables.
    
2. **Initialization Vector (IV)**: This is a random value used during encryption to ensure that even if the same data is encrypted multiple times, the resulting ciphertext is different each time.
    
3. **Base64 Encoding**: After encrypting, we use Base64 encoding to make the ciphertext URL safe. This is important if the encrypted data needs to be passed in URLs or APIs.
    
4. **URL Safety**: The encrypted data is formatted for safe transmission over the web by replacing certain characters (`+` to `-`, `/` to `_`) and removing padding (`=`).
    

### Why Use AES Encryption?

AES encryption is one of the most secure encryption standards available and is widely used for securing sensitive data. Here are a few benefits:

* **Fast**: AES is highly optimized and fast, making it suitable for real-time encryption in applications.
    
* **Secure**: It has been extensively tested and is trusted by industries like banking and government.
    
* **Versatile**: It can be used for encrypting everything from small strings to large files.
    

### Best Practices for Using AES Encryption

1. **Keep your secret key secure**: Never hard-code your secret key in the source code. Instead, store it in environment variables or a secure vault.
    
2. **Use a random IV**: Always use a new, random IV for each encryption operation to ensure that the same plaintext doesn't result in the same ciphertext.
    
3. **Handle errors properly**: Always include error handling in your decryption process. If the key or data is invalid, return a meaningful message.
    

### Conclusion

AES encryption with CryptoJS is a powerful tool for securing your data in JavaScript applications. By encrypting sensitive information such as user data or API keys, you can ensure that your web or mobile app is more secure.

With this easy-to-follow guide, you're now equipped to implement encryption and decryption in your projects. Whether you're securing API calls, user data, or sensitive business information, AES is a reliable method to protect your data.