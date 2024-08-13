# BHL Open Data

The Biodiversity Heritage Library (BHL) is the world's largest open access digital library for biodiversity literature and archives. BHL is revolutionizing global research by providing free, worldwide access to knowledge about life on Earth.

To document Earth's species and understand the complexities of swiftly-changing ecosystems in the midst of a major extinction crisis and widespread climate change, researchers need something that no single library can provide - access to the world's collective knowledge about biodiversity. While natural history books and archives contain information that is critical to studying biodiversity, much of this material is available in only a handful of libraries globally. Scientists have long considered this lack of access to biodiversity literature as a major impediment to the efficiency of scientific research.

## Open Data
BHL's data is structured in a simple format of three buckets of content hosted on S3 at Amazon Web Services. The first bucket contains the JPEG 2000 page images of the scanned content at BHL and is organized by an identifying string. The second bucket is the text content of the page images, sourced either from automated Optical Character Recognition (OCR) software or manual transcription efforts. The third bucket of content is data contained in one of several data export files (in tab-separated format) that contain the majority of BHL's data. The files, images and OCR are all logically connected through identifiers and ID numbers.

The files are organized via the following structure:

```
bhl-open-data/
    images/
        [BarCode]/
            [BarCode]_0000.jp2
            [BarCode]_0001.jp2
            [...]
            [BarCode]_[####].jp2
    ocr/
        [ItemID]/
            [ItemID]-[PageID]-0000.txt
            [ItemID]-[PageID]-0001.txt
            [...]
            [ItemID]-[PageID]-[####].txt
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

### Images

Images are stored as JPEG 2000 files with some amount of compression applied to retain a balance of quality and size. Generally BHL strives for 300 DPI or better resolution. Alternate file formats or sizes are not supplied at this time. 

### OCR

OCR is stored as individual text files mostly in parallel to the page images. 

### Data

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

## Using the Data

From the Item file, the `BarCode` field is used to create the S3 path or URL to the image file. Pages are numbered sequentially and do not skip any numbers. Due to historical inconsistencies, the first image from an item may be number `[BarCode]_0000.jp2` or `[BarCode]_0001.jp2`.

* S3 Path: `s3://bhl-open-data/images/[ItemID]/[ItemID]_0000.jp2`
* Web URL: `https://bhl-open-data.s3.amazonaws.com/images/[ItemID]/[ItemID]_0000.jp2`

Using the `ItemID` field from the Item file (zero-padded to six digits) and the `PageID` field from the Page file (zero-padded to eight digits), the path to the OCR content for a given page is constructed similarly: 

* S3 path: `s3://bhl-open-data/ocr/[ItemID]/[ItemID]-[PageID]-0000.txt`
* Web URL: `https://bhl-open-data.s3.amazonaws.com/ocr/[ItemID]/[ItemID]-[PageID]-0000.txt`

## Update Frequency

Content is updated monthly, but due to conetent updates and new content ingest, this dataset may be out of date for up to a month compared to BHL's live content.

## Copyright Notes

While most content on BHL is either public domain or licensed through Creative Commons, some content from BHL remains in copyright with no conditions for reuse and is suppressed from this repository. The item.txt and part.txt files contain details on copyright status and Creative Commons licenses for all items in BHL.
