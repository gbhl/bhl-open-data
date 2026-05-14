# Special Notes

The following describes some special cases to be considered when using combining the data from the TSV Data files and Images or OCR.

## OCR File organization

There is a mixed relationship between Items and Parts, OCR files must be accessed in different ways.

1. An Item may have no Parts at all.
2. An Item may have Parts defined within it.
3. A Part may be its own Item.

### An item with no Parts

An Item that has a `BarCode` in `item.txt` will have an Image an an associated OCR file. Example:

**Item ID:** 346951 (https://www.biodiversitylibrary.org/item/346951)<br>
**BarCode:** CAT109916943238207426<br>
**Page ID:** 65077706 (https://www.biodiversitylibrary.org/page/65077706)<br>
**Item Sequence:** 14<br>

https://bhl-open-data.s3.us-east-2.amazonaws.com/images/CAT109916943238207426/CAT109916943238207426_0014.jp2

https://bhl-open-data.s3.us-east-2.amazonaws.com/ocr/item-346951/item-346951-65077706-0014.txt

### An Item that has Parts defined within it

Not all Parts have OCR. If a part **does not have** a `BarCode` in `part.txt`, it's parent Item will have a `BarCode` in `item.txt` 
that is used to get the OCR prefixed with `item`. Example:

**Item ID:** 21356 (https://www.biodiversitylibrary.org/item/21356#page/127/mode/1up)<br>
**Part ID:** 248 (https://www.biodiversitylibrary.org/part/248)<br>
**BarCode:** journalofhymenop12n2inte<br>
**Page ID:** 2839616 (https://www.biodiversitylibrary.org/page/2839616)<br>
**Item Sequence:** 127<br>
**Part Sequence:** 1<br>

https://bhl-open-data.s3.us-east-2.amazonaws.com/images/journalofhymenop12n2inte/journalofhymenop12n2inte_0127.jp2

https://bhl-open-data.s3.us-east-2.amazonaws.com/ocr/item-021356/item-021356-02839616-0127.txt

https://bhl-open-data.s3.us-east-2.amazonaws.com/ocr/part-000248/part-000248-02839616-0001.txt **<-- Does not exist**

### A Part that is its own Item

Some Parts do not have parent items. If an part has a `BarCode` in `part.txt` then it will have a correspoding OCR file prefixed with `part`. There is no corresponding OCR for the Item. In `item.txt` these will have a BarCode that looks like `vi210914v100201120250316010124` (regex `/^vi\d{6}v/`)

**Item ID:** 336513 (https://www.biodiversitylibrary.org/itemdetails/336513)<br>
**Part ID:** 98691 (https://www.biodiversitylibrary.org/part/98691)<br>
**BarCode:** giantresinbeema1hino<br>
**Page ID:** 64253797 (https://www.biodiversitylibrary.org/page/64253797)<br>
**Item Sequence:** N/A<br>
**Part Sequence:** 1<br>

https://bhl-open-data.s3.us-east-2.amazonaws.com/images/giantresinbeema1hino/giantresinbeema1hino_0001.jp2

https://bhl-open-data.s3.us-east-2.amazonaws.com/ocr/item-336513/item-336513-064253797-0001.txt **<-- Does not exist**

https://bhl-open-data.s3.us-east-2.amazonaws.com/ocr/part-098691/part-098691-064253797-0001.txt 

