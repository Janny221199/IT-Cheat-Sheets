# iCloud Media
1. make sure macBook is set to download all media in full quality (Photos -> Settings -> iCloud)
   ![431](attachments/Pasted%20image%2020260819015259.png)
2. make sure all media is downloaded and synced
   ![](attachments/Pasted%20image%2020260819015318.png)
Do all 3 Export Types:
## Export Normal
- keeps the edits (filter, shortened Videos, adjusted size, Key-Image of Live-Photo and so on...) of a photo (baked into exported files - NOT reversible to original)
- does NOT keep albums, favourites, faces and so on
- processes file to e.g. jpeg
- maybe looses a bit of quality (but you can set maximum quality)

1. select all media
2. File -> Export-> Export X Items
3. make sure to select the same options (File Format, Quality, Info/Metadata, File Name etc.) -> select Folder `YYYYMMDD_Backup_iCloud_Media_Export_Normal`
   ![388](attachments/Pasted%20image%2020260819014854.png)
4. cd .../parent
5. Run: `zip -r -X YYYYMMDD_Backup_iCloud_Media_Export_Normal.zip YYYYMMDD_Backup_iCloud_Media_Export_Normal -x "*.DS_Store" "__MACOSX/*"`

## Export Unmodified Original
- edits (filter, shortened Videos, adjusted size, Key-Image of Live-Photo and so on...) NOT applied - get untouched original
- does NOT keep albums, favourites, faces and so on
- might get 2 files (e.g. for Live Photo: image and video)
- full quality

1. select all media
2. File -> Export -> Export Unmodified Original For X Items
3. make sure to select the same options -> select Folder `YYYYMMDD_Backup_iCloud_Media_Export_Unmodified_Original`
   ![529](attachments/Pasted%20image%2020260819190926.png)
4. cd .../parent
5. Run: `zip -r -X YYYYMMDD_Backup_iCloud_Media_Export_Unmodified_Original.zip YYYYMMDD_Backup_iCloud_Media_Export_Unmodified_Original -x "*.DS_Store" "__MACOSX/*"`

## Export `Photos Library.photoslibrary`
- keeps the edits (filter, shortened Videos, adjusted size, Key-Image of Live-Photo and so on...) of a photo (additional to the original files - reversible to original)
- keep albums, favourites, faces and so on
- full quality
- ONLY usable for macOS Photos App

I decided to save it as Disk-Image instead of a zip file because of data integrity and better import option:
1. QUIT Photos App!!
2. Run:
```cmd
  hdiutil create -srcfolder "$HOME/Pictures/Photos Library.photoslibrary" -format UDZO -encryption AES-256 "$HOME/Downloads/20260819_Backup_PhotosLibrary.dmg"
```
3. save .dmg file