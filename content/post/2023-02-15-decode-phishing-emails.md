---
author: Tristan Madden
categories: [Python]
date: 2023-02-15
tags: [phishing]
title: Decode Phishing Emails
---

<h2>Decode HEX strings</h2>

```Python
import re

def decode_hex(match):
    hex_str = match.group(0)[2:]
    decoded = bytes.fromhex(hex_str).decode('ascii')
    return decoded

with open('input.html', 'r') as file:
    content = file.read()
    decoded_content = re.sub(r'\\x[0-9a-fA-F]{2}', decode_hex, content)

with open('output.html', 'w') as file:
    file.write(decoded_content)
```

In this program, the re module is used to perform regular expression matching and replacement. The decode_hex function is defined to take a regular expression match object as input, extract the hexadecimal string from the match, decode it to regular ASCII characters, and return the decoded string.

The with statement is used to open the input.html file and read its contents into the content variable. The re.sub function is then used to replace all occurrences of the regular expression r'\\x[0-9a-fA-F]{2}' with the result of calling the decode_hex function on each match. This regular expression matches any sequence of characters that starts with \\x and is followed by two hexadecimal digits.

The resulting decoded content is written to an output.html file using the open function and the 'w' write mode.

Note that this program assumes that the input HTML file contains only hexadecimal representations of ASCII characters that are encoded using the \\x notation. If there are other types of encodings present in the file, they will not be handled by this program.

<h2>Decode HEX strings</h2>

<h2></h2>

```Python
# Custom function to decode URL-encoded characters
def url_decode(string):
    i = 0
    result = ""
    while i < len(string):
        if string[i] == '%':
            try:
                result += chr(int(string[i+1:i+3], 16))
                i += 3
            except ValueError:
                result += string[i:i+3]
                i += 3
        else:
            result += string[i]
            i += 1
    return result

# Load the input HTML file
with open('output.html', 'r') as input_file:
    html = input_file.read()

# Decode the URL-encoded characters
decoded_html = url_decode(html)

# Write the decoded HTML to a new file
with open('decoded.html', 'w') as output_file:
    output_file.write(decoded_html)

```
This script defines a custom function called url_decode() that takes a URL-encoded string as input and returns the decoded string. The url_decode() function uses a while loop to iterate through the input string character by character. If a percent sign is encountered, the function uses the int() function to convert the following two characters to a hexadecimal value, and then uses the chr() function to convert the hexadecimal value to an ASCII character. If there is an invalid literal for int(), the function simply adds the three characters to the output string as-is. The url_decode() function then returns the decoded string.

The rest of the script is similar to the previous example. It loads the input HTML file, decodes the URL-encoded characters using the url_decode() function, and writes the decoded HTML to a new file named decoded.html. Note that this script also assumes that the input HTML file is named output.html and is in the same directory as the Python script. You may need to adjust the file paths in the script to match your specific use case.