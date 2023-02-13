# PicoCTF

## patchme.py

into that code password is unncessary so, comment some unuseful code lines.

## rail-fence

search on the google and i got the code to decrypt the message.

[Reference code](https://www.geeksforgeeks.org/rail-fence-cipher-encryption-decryption/)

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

## credstuff

`tar -xf leak.tar`

`cat usernames.txt`

`cat passwords.txt`

`cat usernames.txt | grep -n "cultiris"`

`sed -n 378,378p passwords.txt`

I got a flag but it is encrypted. (Algo:ROT13)

## Enhance

download the image and extract all the string using string command and I got the flag

`strings drawing.flag.svg`

## file-run1

just execute the file and i got the flag

## file-run2

execute the file with "Hello!" arrgument

## safe opener

download the file and get the encoded text which is look like an base64

so decode it and we get our flag.

## substitution2

Not solved yet

## unpackme

change into the program add print line to print output and we the flag

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

## basic-mod2

mode inverse get the flag from that

## Search source

`wget -m [url]`

flag was in style.css file

## Forbidden Path

just change/add the hearder

`X-Origination-IP: 127.0.0.1`

and add the file path

`/../../../../../../../../../../flag.txt`

above file should be in url encoded.

## Secrets

using gobuster tool I got my first hidden path which name was *secret*. After using again gobuster I got *hidden* path there was a secret directory path mention in html source code which was *superhidden*. there was our flag is in web page's source code.

## Roboto Sans

*flag1.txt* and *js/myfile.txt* file name got from the *robots.txt* page which is in base64 encoded.

and our flag into the *js/myfile.txt* file.

## SQL Direct

just use PostgreSQL.

1. `\dt+` : By using this command we got table list

2. `SELECT * FROM flags;` : after enter this command we could get our flag.

## SQLiLite

username: admin
password: ' OR 1=1-- -

and we got our flag.

## Power Cookie

intersept the request and change a cookie flag value to 1

*Cookie: isAdmin=1*

and we get our flag.

## GDB Test Drive

just follow the instruction and we will get the flag.

## bloat.py

open the python program and flow the code and we can get the password from the code just copy that string and paste into another python file just print the password.

## Fresh java

decompile the class file and remove the unwanted things using text ediotor and print the flag in reverse order.

## Lookey here

just open the text file and search or use find tool: "pico{".

## Reduction gone wrong

open the pdf and select the black content

## Sleuthkit intro

follow the instruction and get the flag

```
gunzip disk.img.gz
mmls -a disk.img
```

Enter the linux image length in nc command.

## buffer overflow 0

`\x31\xc0\x31\xdb\x31\xd2\x53\x68\x55\x6e\x69\x0a\x68\x64\x55`

enter the shellcode and get we got our flag.