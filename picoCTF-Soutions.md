# PicoCTF

## patchme.py

### Solution
- The password check in the code is unnecessary.
- Simply comment out the lines related to password verification.

---

## Rail-Fence

### Solution
- The challenge uses a Rail-Fence cipher. I found a decryption method online and used it to decode the message.

**Reference:**
- [Rail-Fence Cipher Decryption Code](https://www.geeksforgeeks.org/rail-fence-cipher-encryption-decryption/)

```cpp
#include <iostream>

using namespace std;

string decryptRailFence(string cipher, int key)
{
    // create the matrix to cipher plain text
    // key = rows , length(text) = columns
    char rail[key][cipher.length()];
 
    // filling the rail matrix to distinguish filled
    // spaces from blank ones
    for (int i=0; i < key; i++)
        for (int j=0; j < cipher.length(); j++)
            rail[i][j] = '\n';
 
    // to find the direction
    bool dir_down;
 
    int row = 0, col = 0;
 
    // mark the places with '*'
    for (int i=0; i < cipher.length(); i++)
    {
        // check the direction of flow
        if (row == 0)
            dir_down = true;
        if (row == key-1)
            dir_down = false;
 
        // place the marker
        rail[row][col++] = '*';
 
        // find the next row using direction flag
        dir_down?row++ : row--;
    }
 
    // now we can construct the fill the rail matrix
    int index = 0;
    for (int i=0; i<key; i++)
        for (int j=0; j<cipher.length(); j++)
            if (rail[i][j] == '*' && index<cipher.length())
                rail[i][j] = cipher[index++];
 
 
    // now read the matrix in zig-zag manner to construct
    // the resultant text
    string result;
 
    row = 0, col = 0;
    for (int i=0; i< cipher.length(); i++)
    {
        // check the direction of flow
        if (row == 0)
            dir_down = true;
        if (row == key-1)
            dir_down = false;
 
        // place the marker
        if (rail[row][col] != '*')
            result.push_back(rail[row][col++]);
 
        // find the next row using direction flag
        dir_down?row++: row--;
    }
    return result;
}

int main(void)
{
    for (int i = 1; i <= 10; i++) {
        //Now decryption of the same cipher-text
        cout << "----" << endl;
        cout << decryptRailFence("Ta _7N6DDDhlg:W3D_H3C31N__0D3ef sHR053F38N43D0F i33___NA",i) << endl;
        cout << "----" << endl;
    }
    return 0;
}
```

---

## credstuff

### Solution
- Extract the archive and inspect the files:
  ```bash
  tar -xf leak.tar
  cat usernames.txt
  cat passwords.txt
  ```
- Search for the specific user:
  ```bash
  cat usernames.txt | grep -n "cultiris"
  ```
- Retrieve the corresponding password:
  ```bash
  sed -n 378,378p passwords.txt
  ```
- The flag is encrypted using ROT13. Decode it to get the flag.

---

## Enhance

### Solution
- Extract strings from the provided image file:
  ```bash
  strings drawing.flag.svg
  ```
- The extracted text contains the flag.


---

## file-run1

### Solution
- Simply execute the file to reveal the flag.

---

## file-run2

### Solution
- Execute the file with a "Hello!" argument to obtain the flag.

---

## safe opener

### Solution
- Download the provided file and extract the encoded text.
- The text appears to be Base64 encoded. Decode it to reveal the flag.

---

## unpackme

### Solution
- Modify the program to print the decrypted payload.

Reference Code and [link](https://cryptography.io/en/latest/fernet/):

```python
from cryptography.fernet import Fernet

key = Fernet.generate_key()

f = Fernet(key)

token = f.encrypt(b"my deep dark secret")

token
b'...'

f.decrypt(token)
b'my deep dark secret'
```

*Before*

```python
import base64
from cryptography.fernet import Fernet



payload = b'gAAAAABiMD09KmaS5E6AQNpRx1_qoXOBFpSny3kyhr8Dk_IEUu61Iu0TaSIf8RCyf1LJhKUFVKmOt2hfZzynRbZ_fSYYN_OLHTTIRZOJ6tedEaK6UlMSkYJhRjAU4PfeETD-8gDOA6DQ8eZrr47HJC-kbyi3Q5o3Ba28mutKCAkwrqt3gYOY9wp3dWYSWzP4Tc3NOYWfu-SJbW997AM8GA-APpGfFrf9f7h0VYcdKOKu4Vq9zjJwmTG2VXWFET-pkF5IxV3ZKhz36L5IvZy1dVZXqaMR96lovw=='

key_str = 'correctstaplecorrectstaplecorrec'
key_base64 = base64.b64encode(key_str.encode())
f = Fernet(key_base64)
plain = f.decrypt(payload)
exec(plain.decode())
```

*After*

```python
import base64
from cryptography.fernet import Fernet



payload = b'gAAAAABiMD09KmaS5E6AQNpRx1_qoXOBFpSny3kyhr8Dk_IEUu61Iu0TaSIf8RCyf1LJhKUFVKmOt2hfZzynRbZ_fSYYN_OLHTTIRZOJ6tedEaK6UlMSkYJhRjAU4PfeETD-8gDOA6DQ8eZrr47HJC-kbyi3Q5o3Ba28mutKCAkwrqt3gYOY9wp3dWYSWzP4Tc3NOYWfu-SJbW997AM8GA-APpGfFrf9f7h0VYcdKOKu4Vq9zjJwmTG2VXWFET-pkF5IxV3ZKhz36L5IvZy1dVZXqaMR96lovw=='

key_str = 'correctstaplecorrectstaplecorrec'
key_base64 = base64.b64encode(key_str.encode())
print(key_base64)
f = Fernet(key_base64)
plain = f.decrypt(payload)
print(plain)
exec(plain.decode())
```

---

## basic-mod2

### Solution
- Use modular inverse to retrieve the flag.

---

## Search source

### Solution
- Download the entire site using `wget`:
  ```bash
  wget -m [url]
  ```
- The flag was found in the `style.css` file.

---

## Forbidden Path

### Solution
- Modify the HTTP request header to include:
  ```bash
  X-Origination-IP: 127.0.0.1
  ```
- Access the file path using:
  ```bash
  /../../../../../../../../../../flag.txt
  ```
- Ensure the file path is URL encoded.

---

## Secrets

### Solution
- Use `gobuster` to discover hidden paths:
  ```bash
  gobuster dir -u [url] -w /path/to/wordlist
  ```
- Hidden paths such as `secret`, `hidden`, and `superhidden` contain clues leading to the flag in the source code.

---

## Roboto Sans

### Solution
- Retrieve file names from `robots.txt`, including `flag1.txt` and `js/myfile.txt`.
- Decode the Base64 encoded content from `js/myfile.txt` to reveal the flag.

---

## SQL Direct

### Solution
- Use PostgreSQL syntaxs to access the database:
  ```sql
  \dt+
  SELECT * FROM flags;
  ```

---

## SQLiLite

### Solution
- Use SQL injection with the following credentials:
  ```
  username: admin
  password: ' OR 1=1-- -
  ```

---

## Power Cookie

### Solution
- Intercept the request and modify the cookie value to:
  ```
  Cookie: isAdmin=1
  ```
- The modified request reveals the flag.

---

## GDB Test Drive

### Solution
- Follow the provided instructions within the GDB session to uncover the flag.

---

## bloat.py

### Solution
- Open the Python script and trace the logic to identify the password.
- Copy the relevant string to another Python file and print the password.

---

## Fresh Java

### Solution
- Decompile the Java class file and modify it to output the flag in reverse order.

---

## Lookey Here

### Solution
- Open the text file and search for the flag using:
  ```bash
  grep "pico{" filename.txt
  ```
---

## Reduction Gone Wrong

### Solution
- Open the PDF and highlight the blacked-out sections to reveal hidden text containing the flag.

---

## Sleuthkit Intro

### Solution
- Follow these commands to analyze the disk image and retrieve the flag:
  ```bash
  gunzip disk.img.gz
  mmls -a disk.img
  ```

- Enter the linux image length in nc command.

---

## Buffer Overflow 0

### Solution
- Enter the provided shellcode to trigger the buffer overflow and obtain the flag:

`\x31\xc0\x31\xdb\x31\xd2\x53\x68\x55\x6e\x69\x0a\x68\x64\x55`
