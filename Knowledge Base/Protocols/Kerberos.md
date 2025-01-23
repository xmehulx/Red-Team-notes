# #kerberos 
# TGT Encryption Type
A TGT is only read by domain controllers in the issuing domain.  As a result, the encryption type of the TGT only needs to be supported by the domain controllers.  Once our domain functional level (DFL) is 2008 or higher, our KRBTGT account will always default to AES encryption.  For all other account types (user and computer) the selected encryption type is determined by the **`msDS-SupportedEncryptionTypes`** attribute on the account.
## msDS-SupportedEncryptionTypes
This attribute uses a single HEX value to define which encryption types are supported. We could calculate the value based on this [article](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-kile/6cfc7b50-11ed-4b4d-846d-6f08f0812919?redirectedfrom=MSDN) or use the following decoder ring:

| **Decimal Value** | **Hex Value** | **Supported Encryption Types**                                                       |
| ----------------- | ------------- | ------------------------------------------------------------------------------------ |
| 0                 | 0x0           | Not defined - defaults to RC4_HMAC_MD5                                               |
| 1                 | 0x1           | DES_CBC_CRC                                                                          |
| 2                 | 0x2           | DES_CBC_MD5                                                                          |
| 3                 | 0x3           | DES_CBC_CRC, DES_CBC_MD5                                                             |
| 4                 | 0x4           | RC4                                                                                  |
| 5                 | 0x5           | DES_CBC_CRC, RC4                                                                     |
| 6                 | 0x6           | DES_CBC_MD5, RC4                                                                     |
| 7                 | 0x7           | DES_CBC_CRC, DES_CBC_MD5, RC4                                                        |
| 8                 | 0x8           | AES 128                                                                              |
| 9                 | 0x9           | DES_CBC_CRC, AES 128                                                                 |
| 10                | 0xA           | DES_CBC_MD5, AES 128                                                                 |
| 11                | 0xB           | DES_CBC_CRC, DES_CBC_MD5, AES 128                                                    |
| 12                | 0xC           | RC4, AES 128                                                                         |
| 13                | 0xD           | DES_CBC_CRC, RC4, AES 128                                                            |
| 14                | 0xE           | DES_CBC_MD5, RC4, AES 128                                                            |
| 15                | 0xF           | DES_CBC_CBC, DES_CBC_MD5, RC4, AES 128                                               |
| 16                | 0x10          | AES 256                                                                              |
| 17                | 0x11          | DES_CBC_CRC, AES 256                                                                 |
| 18                | 0x12          | DES_CBC_MD5, AES 256                                                                 |
| 19                | 0x13          | DES_CBC_CRC, DES_CBC_MD5, AES 256                                                    |
| 20                | 0x14          | RC4, AES 256                                                                         |
| 21                | 0x15          | DES_CBC_CRC, RC4, AES 256                                                            |
| 22                | 0x16          | DES_CBC_MD5, RC4, AES 256                                                            |
| 23                | 0x17          | DES_CBC_CRC, DES_CBC_MD5, RC4, AES 256                                               |
| 24                | 0x18          | AES 128, AES 256                                                                     |
| 25                | 0x19          | DES_CBC_CRC, AES 128, AES 256                                                        |
| 26                | 0x1A          | DES_CBC_MD5, AES 128, AES 256                                                        |
| 27                | 0x1B          | DES_CBC_MD5, DES_CBC_MD5, AES 128, AES 256                                           |
| 28                | 0x1C          | RC4, AES 128, AES 256                                                                |
| 29                | 0x1D          | DES_CBC_CRC, RC4, AES 128, AES 256                                                   |
| 30                | 0x1E          | DES_CBC_MD5, RC4, AES 128, AES 256                                                   |
| 31                | 0x1F          | DES_CBC_CRC, DES_CBC_MD5, RC4-HMAC, AES128-CTS-HMAC-SHA1-96, AES256-CTS-HMAC-SHA1-96 |