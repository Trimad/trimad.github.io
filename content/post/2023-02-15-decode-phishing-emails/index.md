---
author: Tristan Madden
categories: [Python]
date: 2023-02-15
lastmod: 2023-02-15
summary: I've notice that a lot of phishing emails use hexadecimal strings to obfuscate their JavaScript. These are some Python scripts useful for identifying where form actions are POSTing to. 
tags: [phishing]
title: Decode Phishing Emails
thumbnail: "thumbnail.png"
toc: true
usePageBundles: true
---

## atob() obfuscation

```JavaScript
let input =
  "PGh0bWw+CjxoZWFkPgo8ZGl2IGNsYXNzPSIiIHN0eWxlPSJkaXNwbGF5Om5vbmU7Ij48aDMgaWQ9Im1xZnZBRFd1WmFTeEVSUHZYT2pZIiBjbGFzcz0iYlBFUUNlWWNUVHdSU1ZjZUJnTGUiIHN0eWxlPSJkaXNwbGF5Om5vbmUiPlRRaFpZdUFwTTwvaDM+PC9kaXY+PC9oZWFkPgo8Ym9keT4KPGRpdiBjbGFzcz0iIiBzdHlsZT0iZGlzcGxheTpub25lOyI+PGJ1dHRvbiBpZD0ieFVMell4ZmZwcmpIQ3F4QnpseHEiIGNsYXNzPSJHZ2FWRkxDTWRYT2JQbWR6TmZ6YiIgc3R5bGU9ImRpc3BsYXk6bm9uZSI+TUhuamNvU25TPC9idXR0b24+PC9kaXY+CjxpbnB1dCBjbGFzcz0iUmpVZ3dwRnNKUmlLIiB0eXBlPSJoaWRkZW4iIGlkPSJiNjR1IiB2YWx1ZT0iYUhSMGNITTZMeTluY25Wd2IyUnBjMkZ0TG1OdmJTOXJiMnh3YjNNeEwyaHZjM1F4TVRJdlpqZGhZamN6T0M1d2FIQT0iPjwvaW5wdXQ+CjxkaXYgY2xhc3M9IiIgc3R5bGU9ImRpc3BsYXk6bm9uZTsiPjxkaXYgaWQ9ImlQT2VrbHNCZ3N3TWh2Y05BWXZwIiBjbGFzcz0iT05kblRKVkRiWnZ0UlFZREJKY2QiIHN0eWxlPSJkaXNwbGF5Om5vbmUiPmVKUVRhWEhuc3NDTzwvZGl2PjwvZGl2Pgo8aW5wdXQgY2xhc3M9IjRJaGYyVGRoaFhjcSIgdHlwZT0iaGlkZGVuIiBpZD0iY29uZiIgdmFsdWU9ImV5SmlZV05ySWpvaVpHVm1ZWFZzZENJc0luUnBkR3hsSWpvaVpHVm1ZWFZzZENJc0ltTmhjSFJwYjI0aU9pSmtaV1poZFd4MEluMD0iPjwvaW5wdXQ+CjxkaXYgY2xhc3M9IiIgc3R5bGU9ImRpc3BsYXk6bm9uZTsiPjx0ciBpZD0iYXVZT1NCZVJoUFBmYUdnZWRCSmwiIGNsYXNzPSJDWGdhd2ZodnpGcXB3TUZHT3RYTiIgc3R5bGU9ImRpc3BsYXk6bm9uZSI+a1dHT2Nac2dEbXRidW9CTTwvdHI+PC9kaXY+PGlucHV0IHR5cGU9ImhpZGRlbiIgY2xhc3M9Ik1laWZvQ1FGWUhmNSIgaWQ9ImI2NGUiIHZhbHVlPSJhMlJoYm1sbGJITkFZM05pYldsdVl5NWpiMjA9Ij48L2lucHV0PiA8ZGl2IGNsYXNzPSIiIHN0eWxlPSJkaXNwbGF5Om5vbmU7Ij48ZGl2IGlkPSJ1YWR1aGFudUJCa3BXUFlHWW9RSyIgY2xhc3M9InpCUHFmbkxkeXduc1V3ZVBSUmtBIiBzdHlsZT0iZGlzcGxheTpub25lIj5naHU8L2Rpdj48L2Rpdj4KPGlucHV0IHR5cGU9ImhpZGRlbiIgaWQ9ImJ0eXBlIiBjbGFzcz0iMGlEcmoydFNhanFVIiB2YWx1ZT0iYjJabWFXTmwiPgo8Ym9keSBzdHlsZT0iIiBvbmxvYWQ9ImV2YWwoYXRvYignZG1GeUlHbHpYMjUxYkd3Z1BTQmtiMk4xYldWdWREc2dZMjl1YzNRZ2MyTnlJRDBnYVhOZmJuVnNiQzVqY21WaGRHVkZiR1Z0Wlc1MEtDSnpZeUl1WTI5dVkyRjBLQ0p5YVhCMElpa3BPdzBLSUhkb2FXeGxLSFJ5ZFdVcGV5QjJZWElnYm05MFgybHpJRDBnYzJOeU95QnViM1JmYVhNdWMzSmpQV0YwYjJJb0ltRklVakJqU0UwMlRIazVibU51Vm5kaU1sSndZekpHZEV4dFRuWmlVemx5WWpKNGQySXpUWGhNTW1oMll6TlJlRTFVU1haWlYxSjBZVmMwZG1GdVRYWmlWMjkxWTBkb2QxQXlSbmxRVjBsNVYyMHhhRll3TlhNaUtUc05DaUJwYzE5dWRXeHNMbWhsWVdRdVlYQndaVzVrUTJocGJHUW9ibTkwWDJsektUdGljbVZoYXp0aGJHVnlkQ2dpVkdobElHUnZiU0lwT3lCMllYSWdjSEp6WDNRZ1BTQm1ZV3h6WlRzZ0lHbG1LSEJ5YzE5MEtYdDNhVzVrYjNjdWFHRnphRDBpYUdWb1pDSjlmVHM9JykpIgo8L2JvZHk+CjwvaHRtbD4=";

let matches = [];

function setup() {
  createCanvas(400, 400);
  noLoop();

  isBase64(input);
  repeatedlyDecode(input);

  print(matches);
}

function draw() {
  background(220);
}

function repeatedlyDecode(str) {
  if (isBase64(str)) {
    const decoded = atob(str);
    const regex = /(['"])(?:(?=(\\?))\2.)*?\1/g; // regex to match quoted substrings
    let match;
    while ((match = regex.exec(decoded)) !== null) {
      //print(match)
      matches.push([match[0],decode(match[0])]); // add the match to the array of matches
      repeatedlyDecode(decode(match[0]));
    }
  }
}

function decode(str) {
  if (typeof str !== 'string') {
    return null;
  }
  // trim single or double quotes from beginning and end of str
  str = str.replace(/^['"]|['"]$/g, '');
  if (isBase64(str)) {
    return atob(str);
  }
  return null;
}


function isBase64(str) {
  const base64Regex = /^[A-Za-z0-9+/=]+$/;
  if (!base64Regex.test(str)) {
    return false;
  }
  try {
    const decoded = atob(str);
    return true;
  } catch (e) {
    return false;
  }
}

```

I've notice that a lot of phishing emails use hexadecimal strings to obfuscate their JavaScript. These are some Python scripts useful for identifying where form actions are POSTing to. 

## HEX strings

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

## URI-encoded

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