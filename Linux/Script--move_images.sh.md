Script: move_images.sh

  #!/bin/bash
# Move all image files from Downloads to Pictures/Images

SOURCE="$HOME/Downloads"
DEST="$HOME/Pictures/Images"

mkdir -p "$DEST"
mv "$SOURCE"/*.jpg "$SOURCE"/*.jpeg "$SOURCE"/*.png "$SOURCE"/*.gif "$DEST" 2&gt;/dev/null


copy to text note, save with `.sh` extension
