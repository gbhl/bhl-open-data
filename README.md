# BHL Open Data

The Biodiversity Heritage Library (BHL) is the world's largest open access digital library for biodiversity literature and archives. BHL is revolutionizing global research by providing free, worldwide access to knowledge about life on Earth.

To document Earth's species and understand the complexities of swiftly-changing ecosystems in the midst of a major extinction crisis and widespread climate change, researchers need something that no single library can provide - access to the world's collective knowledge about biodiversity. While natural history books and archives contain information that is critical to studying biodiversity, much of this material is available in only a handful of libraries globally. Scientists have long considered this lack of access to biodiversity literature as a major impediment to the efficiency of scientific research.

In 2024, BHL was accepted into the [Amazon AWS Open Data Sponsorship Program](https://aws.amazon.com/opendata/open-data-sponsorship-program/) and has uploaded its metadata, JPEG-2000 images, and OCR to AWS S3 for use by anyone at no cost.

# Open Data

BHL's data is structured in into four "folders" in a bucket at at Amazon Web Services S3. The first folder, "/images/", contains the JPEG 2000 page images of the scanned content at BHL and is organized by an identifying string. The second folder, "/ocr/" contains the text content of the page images, sourced either from automated Optical Character Recognition (OCR) software or manual transcription efforts. The third folder, "/scandata/", contains XML files that describe the pages of the item. The fourth folder, "/data/", contains the data export files (in tab-separated format) that contain the majority of BHL's data. 

The files, images and OCR are all logically connected through identifiers and ID numbers.

```
bhl-open-data/
    images/
        [BarCode]/
            [BarCode]_0001.jp2
            [BarCode]_0002.jp2
            [...]
            [BarCode]_[####].jp2
    ocr/
        item-[ItemID]/
            item-[ItemID]-[PageID]-0001.txt
            item-[ItemID]-[PageID]-0002.txt
            [...]
            item-[ItemID]-[PageID]-[####].txt
        part-[PartID]/
            part-[PartID]-[PageID]-0001.txt
            part-[PartID]-[PageID]-0002.txt
            [...]
            part-[PartID]-[PageID]-[####].txt
    scandata/
        [BarCode]_scandata.xml
        [...]
        [BarCodeN]_scandata.xml
    data/
        title.txt.gz
        item.txt.gz
        page.txt.gz
        part.txt.gz
        creator.txt.gz
        subject.txt.gz
        titleidentifier.txt.gz
        partidentifier.txt.gz
        partcreator.txt.gz
        creatoridentifier.txt.gz
        pagename.txt.gz
        doi.txt.gz
```

## Images

Images are stored as JPEG 2000 files with some amount of compression applied to retain a balance of quality and size. Generally BHL strives for 300 DPI or better resolution. Alternate file formats or sizes are not supplied at this time. 

## OCR

OCR is stored as individual text files in parallel to the page images. The OCR is broken into two sets of files, one for *items* and one for *parts*. *Items* are usually cover-to-cover book-like things while *Parts* are usually individual journal articles. 

ItemIDs and PartIDs are zero-padded to six digits. PageIDs are zero-padded to eight digits. Sequence numbers start with 1.

## Scandata

The Scandata XML files contain page level metadata including page number and page type. The format is the same that is used at the Internet Archive. Little documentation exists, but only the pages marked as `<addToAccessFormats>true</addToAccessFormats>` are included in this AWS data set. 

For this reason, the `leafNum` values in the Scandata XML will not correspond to the Sequence Number of images or OCR. Sequence numbers can be correlated to the scandata file sorting by `leafNum` and looping through them while skipping those pages where `addToAccessFormats` is `false`.

## Data

Data files are described in detail at https://www.biodiversitylibrary.org/data/TSV/BHLExportSchema.pdf but described briefly below. 

* The **Title** table contains bibliographic metadata about the journals and monographs represented in the BHL web portal, as extracted from the contributing library's catalogue at the time of scanning or applied post-scanning. 

* The **Item** table contains information about each bound object (or "book") digitized from a contributing library. For a serial, journal, or multi-volume monograph, an item represents a volume or multiple volumes bound together. For a single-volume monograph an item represents the book. Items are related to Titles.

* The **Page** table contains the metadata about the scanned pages related to an Item.

* The **Part** table contains information about articles/chapters/treatments/etc and are related to an Item.

* The **Creator** table contains the names of the authors of a Title.

* The **Subject** table contains information about subject headings assigned to each Title.

* The **TitleIdentifier** table contains standard identifiers for Titles, as extracted from the contributing library's catalogue at the time of scanning or applied post-scanning. These may include ARKs, OCLC numbers, Wikidata Q numbers, etc.

* The **PartIdentifier** table contains standard identifiers for Parts such as OCLC numbers, Wikidata Q numbers, ISBN, ISSN, ARKs, etc.

* The **CreatorIdentifier** table contains standard identifiers for Creators such as OCLC numbers, Wikidata Q numbers, ISBN, ISSN, ARKs, etc.

* The **PartCreator** table connects Creators to Parts.

* The **PageName** table lists the scientific names that have been identified and the Pages on which those names are found.

* The **DOI** table contains information about Digital Object Identifiers that have been assigned to BHL entities (Titles, Items, or Pages).

# Using the Data

From the `/data/item.txt` file, the `BarCode` field is used to create the S3 path or URL to the image file. Pages are numbered sequentially and do not skip any numbers. The first image from an item is always `[BarCode]_0001.jp2`.

* S3 Path: `s3://bhl-open-data/images/[BarCode]/[BarCode]_0001.jp2`
* Web URL: `https://bhl-open-data.s3.amazonaws.com/images/[BarCode]/[BarCode]_0001.jp2`

Using the `ItemID` field from the `/data/item.txt` file (zero-padded to six digits) and the `PageID` field from the `/data/page.txt` file (zero-padded to eight digits), the path to the OCR content for a given page in an Item is constructed as follows: 

* S3 path: `s3://bhl-open-data/ocr/item-[ItemID]/item-[ItemID]-[PageID]-0001.txt`
* Web URL: `https://bhl-open-data.s3.amazonaws.com/ocr/item-[ItemID]/item-[ItemID]-[PageID]-0001.txt`

Similarly, using the `PartID` field from the `/data/part.txt` file, the connection between Part and Page in the `/data/partpage.txt` file, and the `PageID` field from the `/data/page.txt` file (zero-padded to eight digits), the path to the OCR content for a page in a Part is constructed as follows: 

* S3 path: `s3://bhl-open-data/ocr/item-[ItemID]/item-[ItemID]-[PageID]-0001.txt`
* Web URL: `https://bhl-open-data.s3.amazonaws.com/ocr/item-[ItemID]/item-[ItemID]-[PageID]-0001.txt`

# Update Frequency

Images, OCR and Scandata content is updated weekly. TSV Data files are updated monthly.

# Copyright Notes

While most content on BHL is either public domain or licensed through Creative Commons, some content from BHL remains in copyright with no conditions for reuse and is suppressed from this repository. The item.txt and part.txt files contain details on copyright status and Creative Commons licenses for all items in BHL.

# Special Notes

The `NOTES.md` file describes some special cases to be considered when using combining the data from the TSV Data files, Images, and OCR.

