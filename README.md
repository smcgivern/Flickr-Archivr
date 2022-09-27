# Flickr Archivr

This project downloads your entire Flickr archive and turns it into a static website. 

You can browse the website directly from your filesystem or you can upload the photos to a web server to make it public. The photos are stored on disk organized by date so you can even browse the files just by navigating the folders on your computer.


## Getting Started

Copy `.env.example` to `.env`

Start by creating an app for the Flickr API.
https://www.flickr.com/services/apps/create/apply/

Copy the API Key and Secret to the `.env` file.

Log in to get your access token:

```bash
php scripts/login.php
```

After successfully authorizing the app, it will give you two lines to add to your `.env` file


## Downloading your Flickr Archive

Choose a folder to download everything into. It may require a lot of disk space since it will download every resolution of all your photos. Put the full path to the folder in the `.env` file, make sure to include a trailing slash.

```bash
php scripts/download.php
```

### Folder Structure

Photos are downloaded to a folder structure like the below, based on the date the photo was taken (if available) otherwise the date the photo was uploaded.

```
2022/
    /08/
       /12/
          /XXXXXXXXXXXXXX/
                         /info/photo.json
                         /info/sizes.json
                         /info/exif.json
                         /info/comments.json
                         /sizes/XXXXXXXXXXXXXX_k.jpg
                         /sizes/XXXXXXXXXXXXXX_b.jpg
                         /sizes/....
                         XXXXXXXXXXXXXX.jpg
          /XXXXXXXXXXXXXX/
                         /info/photo.json
                         /info/sizes.json
                         /info/exif.json
                         /info/comments.json
                         /sizes/XXXXXXXXXXXXXX_k.jpg
                         /sizes/XXXXXXXXXXXXXX_b.jpg
                         /sizes/....
                         XXXXXXXXXXXXXX.jpg
       /13/
          /XXXXXXXXXXXXXX/
                         /info/photo.json
                         /info/sizes.json
                         /info/exif.json
                         /info/comments.json
                         /sizes/XXXXXXXXXXXXXX_k.jpg
                         /sizes/XXXXXXXXXXXXXX_b.jpg
                         /sizes/....
                         XXXXXXXXXXXXXX.jpg
```

Each photo gets its own folder at: `YEAR/MONTH/DAY/PHOTO_ID/`. Inside the folder are:

* The original photo stored as `PHOTO_ID.jpg`
* A folder with every other size that Flickr provides, as `sizes/PHOTO_ID_SIZE.jpg`
* A folder with JSON files containing
  * `photo.json` - The photo info including title, description, dates, tags, etc
  * `exif.json` - The complete exif data
  * `sizes.json` - Info about all the sizes of the photo available
  * `comments.json` - If present, all the comments on the photo


## Build Indexes

The indexes are used for various purposes when building the web pages to browse the photos. 

Since photos are stored in a folder by date, this index helps other parts of the system find the photos on disk by just their photo ID. 

```bash
php scripts/indexphotos.php
```

To build an index of all the people and which photos they appear in, run the command below.

```bash
php scripts/indexpeople.php
```

Build an index of all tags in order to create tag pages

```bash
php scripts/indextags.php
```


## Download Albums

Albums (formerly known as photosets), can be downloaded with the command below.

```bash
php scripts/photosets.php
```

This creates a new folder with a subfolder for each album:

```
albums/
      /XXXXXXXXX/album.json
                /photos.json
      /XXXXXXXXX/album.json
                /photos.json    
```

The file `album.json` has the album metadata such as name and modified date. The file `photos.json` contains a list of all the photos in the album.



