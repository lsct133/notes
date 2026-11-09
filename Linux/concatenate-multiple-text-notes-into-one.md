concatenate multiple text notes into one

  HOW TO CONCATENATE MULTIPLE TEXT NOTES INTO ONE ON LINUX, USING THE TERMINAL.

--------------------------------------------------------------------------------


✅ BASIC: CONCATENATE ALL .TXT FILES

cat *.txt &gt; combined.txt

 * This reads all .txt files in the current directory (in shell glob order) and
   writes them to combined.txt.

--------------------------------------------------------------------------------


✅ CONCATENATE SPECIFIC FILES IN A GIVEN ORDER

cat note1.txt note2.txt note3.txt &gt; combined.txt

 * Replace note1.txt, etc., with your actual file names.

--------------------------------------------------------------------------------


🚫 AVOID INCLUDING THE OUTPUT FILE IN THE INPUT LIST

If combined.txt already exists and matches *.txt, it could be included in the
input. To avoid this:

cat $(ls *.txt | grep -v '^combined.txt$') &gt; combined.txt

--------------------------------------------------------------------------------


🧹 BONUS: ADD FILENAME HEADERS BEFORE EACH FILE'S CONTENTS

If you want to know which content came from which file:

for f in *.txt; do
  echo "=== $f ===" &gt;&gt; combined.txt
  cat "$f" &gt;&gt; combined.txt
  echo -e "\n" &gt;&gt; combined.txt
done

This adds a header and a blank line between each file’s content.

--------------------------------------------------------------------------------


CONCATENATING FILES FROM SUB-DIRECTORIES, ACROSS DIFF FILE TYPES

--------------------------------------------------------------------------------


🎯 GOAL:

 * Recursively find files in all sub-directories
 * Include multiple file types (e.g. .txt, .md, .log)
 * Concatenate into a single output file (e.g. combined_output.txt)
 * Optional: Add headers between file contents

--------------------------------------------------------------------------------


📝 BASH SCRIPT: CONCAT_NOTES.SH

#!/bin/bash

# Define output file
output="combined_output.txt"

# Clear previous output
&gt; "$output"

# Define file types to include (space-separated)
file_types=("*.txt" "*.md" "*.log")

# Loop over file types
for pattern in "${file_types[@]}"; do
  # Use find to search recursively for matching files
  while IFS= read -r -d '' file; do
    echo "=== $file ===" &gt;&gt; "$output"
    cat "$file" &gt;&gt; "$output"
    echo -e "\n" &gt;&gt; "$output"
  done &lt; &lt;(find . -type f -name "$pattern" -print0)
done

echo "Done. All contents concatenated into $output"


--------------------------------------------------------------------------------


✅ HOW TO USE IT

 1. Save the script:

nano concat_notes.sh


 2. Paste the code and save (Ctrl+O, then Ctrl+X)
 3. Make it executable:

chmod +x concat_notes.sh


 4. Run it:

./concat_notes.sh


--------------------------------------------------------------------------------


🛠️ OPTIONAL CUSTOMISATION

 * Change file types: Add/remove extensions in the file_types array
   
   file_types=("*.txt" "*.md" "*.log" "*.rst")
   

 * Exclude specific directories:Add -not -path './exclude_dir/*' to the find
   command
   
   find . -type f -name "$pattern" -not -path './backup/*' -print0
   

 * Sort files before concatenation:Add | sort after find:
   
   done &lt; &lt;(find . -type f -name "$pattern" -print0 | sort -z)
   

--------------------------------------------------------------------------------


SKIP BINARY FILES WHEN CONCATENATING

you can add a check in the script that ensures only text files are included.

This can be done using the file command with the --mime or --mime-type option.

--------------------------------------------------------------------------------


🛡️ UPDATED SCRIPT TO SKIP BINARY FILES

#!/bin/bash

# Output file
output="combined_output.txt"

# Clear existing output
&gt; "$output"

# File types to include
file_types=("*.txt" "*.md" "*.log")

# Loop through each file type
for pattern in "${file_types[@]}"; do
  # Find files recursively and handle special characters with -print0
  while IFS= read -r -d '' file; do
    # Check if the file is a text file (not binary)
    mimetype=$(file --mime-type -b "$file")
    if [[ $mimetype == text/* ]]; then
      echo "=== $file ===" &gt;&gt; "$output"
      cat "$file" &gt;&gt; "$output"
      echo -e "\n" &gt;&gt; "$output"
    else
      echo "Skipping binary file: $file"
    fi
  done &lt; &lt;(find . -type f -name "$pattern" -print0)
done

echo "Done. Output saved to $output"


--------------------------------------------------------------------------------


✅ WHAT THIS DOES:

 * Uses file --mime-type to detect if a file is of type text/*
 * Skips files that are not plain text (e.g., application/octet-stream)
 * Prints a message when skipping a binary file

--------------------------------------------------------------------------------


🧪 EXAMPLE OUTPUT DURING RUN:

=== ./notes/todo.txt ===
&lt;file contents&gt;

=== ./logs/error.log ===
&lt;file contents&gt;

Skipping binary file: ./data/core.dump


--------------------------------------------------------------------------------

Let me know if you'd like to log skipped files separately or support more
nuanced MIME checks (e.g., markdown, JSON, XML, etc.).