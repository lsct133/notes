Unzip & zip an appImage

  To decompress an appimage.

1. Make appimage executable:
&nbsp;&nbsp;&nbsp;&nbsp;chmod +x name_of_appimage.AppImage
2. Extract contents:
&nbsp;&nbsp;&nbsp;&nbsp;chmod +x name_of_appimage.AppImage

This create a directory named squashfs-root which contains
the extracted files

To create an appImage from squashfs-root folder.

1. Download the AppImageTool:
&nbsp;&nbsp;&nbsp;&nbsp;https://github.com/AppImage/appimagetool/releases/tag/continuous

2. Create a directory and copy `AppImageTool` and `squashfs-root`
&nbsp;&nbsp;&nbsp;into that new directory. Now open a new terminal inside that
&nbsp;&nbsp;&nbsp;directory and run the following cmd:

./appimagetool-x86_64.AppImage squashfs-root/ MyApp.AppImage
&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;[NOTE!] Replace "MyApp.AppImage" with desired name.

3. After creating the AppImage, make it executable:
&nbsp;&nbsp;chmod +x name_of_appimage.AppImage
&nbsp;&nbsp;&nbsp;&nbsp;
Learn to make an appimages:
https://www.booleanworld.com/creating-linux-apps-run-anywhere-appimage/
