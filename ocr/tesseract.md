# Introduction to tesseract
[TOC]

## Installation
### for windows
#### Step 1 download tesseract-ocr 
http://digi.bib.uni-mannheim.de/tesseract/tesseract-ocr-setup-4.00.00dev.exe

#### Step 2 install tesseract-ocr
Follow default settings except one thing:
Tick the additional language check box and pick the desired language. Here we add Chinese(simplified) and Chinese(traditional).


#### Step 3 set environment variable
The path can be like
C:\Users\[your user name]\AppData\Local\Tesseract-OCR

### for linux

#### Step 1 install tesseract-ocr
```
sudo apt-get install tesseract-ocr
```

#### Step 2 check list of installed langs
```
tesseract --list-langs
```

#### Step 3 search for the list of available language package

```
sudo apt-cache search tesseract-ocr
```

#### Step 4 download languague

```
sudo apt-get install tesseract-ocr-chi-sim
```
## Convert
### in windows

command usage:
> tesseract [image_file_name] [out_file_name] -l [languge]

```
tesseract chi_sim.png chi_result -l chi_sim
```
### in linux

```
tesseract sample1.PNG out -l chi_sim
```

check the result

```
cat out.txt
```

## Train
#### Step 1 download jTessBoxEditor
https://sourceforge.net/projects/vietocr/files/jTessBoxEditor/
#### Step 2 rename the image file
Convert the image file to tif using windows paint tool.
Rename the file name in below pattern
>[lib_name].[font_name].exp[num].tif

In our example we name it 
> libname.fontname.exp0.tif

#### Step 3 generate the box file
generate the box file by below command

```
tesseract libname.fontname.exp0.jpg libname.fontname.exp0 -l chi_sim batch.nochop makebox
```

the box file must be in the same directory of the tif file.
#### Step 4 edit the tif file in jTessBoxEditor

open jTessBoxEditor in linux
```
java -Xms128m -Xmx1024m -jar jTessBoxEditor.jar
```
execute train.bat in the jTessBoxEditor folder

open the box tif file in box editor

#### Step 5 training
```
tesseract libname.fontname.exp0.jpg libname.fontname.exp0 nobatch box.train
```

```
unicharset_extractor libname.fontname.exp0.box
```

new a file called font_properties
inside the file write:

```
normal 0 0 0 0 0
```
which stands for normal font type

```
shapeclustering -F font_properties -U unicharset libname. fontname.exp0.tr 
```

```
mftraining -F font_properties -U unicharset -O unicharset libname.fontname.exp0.tr
```

```
cntraining libname.fontname.exp0.tr
```
five file will be generated 
- unicharset
- inttemp
- pffmtable
- shapetable

normproto rename them as
- libname.unicharset
- libname.inttemp
- libname.pffmtable
- libname.shapetable

```
combine_tessdata libname.
```
#### Step 6 move the trained data into tesseract installation folder

#### Step 7 test the new trained language
```
tesseract libname.fontname.exp0.jpg new_result -l libname
```




## with python

### installation

- python
- pip
- pillow
- pytesseract

### usage example

```python
from PIL import Image
import pytesseract

im = Image.open("sample1.PNG")

text = pytesseract.image_to_string(im, lang = 'chi_sim')

print(text)

```