---
title: Create your markdown files from template
date: 2025-04-12 19:31:59
category: Markdown
toc: true
tags:
  - Markdown
  - Bash
---




Markdown is incredibly useful, especially for text management. I use it extensively for almost anything related to text. Most of the time, I like to keep file structures the same across a project. Using a template file really helps with that. And since Markdown doesn’t have something like `@date` to automatically add the current date, we can just take care of that ourselves too.

To automate the generation of a markdown file from a template, you need `template.md` file and a bash file `create.sh`.

## Markdown template

Basically, a template file is the same as a normal Markdown file, but **without** specific content. I like to add headings or empty tables that are unlikely to change, as well as meta-information such as the date and title. Here's an example of a template file.

```markdown
| Created  |   Title   | Tags |
| -------- | --------- | ---- |
| {{date}} | {{title}} |      |


# Summary
```



## Bash command

To automate the creation of a file, we can use the powerful `hash` command. Here's an example:

```bash
# create.sh
# Check for input
if [ -z "$1" ]; then
  echo "Usage: $0 \"your-title-here\""
  exit 1
fi

# Title name gets from input
raw_title="$*"

# Format input "title with space" => "Title-With-Space"
formated_title=$(echo "$raw_title" | awk '{
  for (i=1; i<=NF; i++) {
    $i = toupper(substr($i,1,1)) tolower(substr($i,2))
  }
  print
}' | tr ' ' '-')

# Get the date
today=$(date +%F)

# Combine the file name
filename="${today}-${formated_title}.md"

# Uncomment if you don't need date or file name in your template
# cp template.md "$filename"

# Uncomment one if you are using date or file name or both in your template
# sed "s/{{date}}/$today/g" template.md > "$filename"
# sed "s/{{title}}/${raw_title//\//-}/g" template.md > "$filename"
# sed -e "s/{{title}}/${raw_title//\//-}/g" -e "s/{{date}}/$today/g" template.md > "$filename"

echo "$filename Created."

```

Depending on whether you want to handle variables in your template, you can uncomment the appropriate option.



## Usage

In the terminal, simply use:

```bash
./create.sh "file name"
```

A file named `YEAR-MONTH-DAY-File-Name.md` will be created in the current folder, with the date and file name pre-filled.

