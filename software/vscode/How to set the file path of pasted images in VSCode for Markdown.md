
The Markdown editor within VS Code is generally quite handy, but there's been one issue that's been bothering me: the placement of pasted images while editing Markdown files. By default, VS Code places pasted images in the same directory as the Markdown file being edited. This often leads to a cluttered directory structure for Markdown files, making it difficult to manage.

However, this issue can be easily resolved with a simple configuration adjustment.

- In VS Code, press `Ctrl +`, to open the settings interface.
- In the search box, type `markdown.copy` and locate `Markdown > Copy Files: Destination`.
- Add a new configuration item with the key "**/*.md" and the value as your desired target path. For instance, if you wish to place images in the `assets` directory under a subdirectory with the same name as the Markdown file, you can set it as `assets/${documentBaseName}/${fileName}`, where `${documentBaseName}` represents the filename of the Markdown file, and `${fileName}` represents the filename of the image.
- Save the settings, and you're all set.

![alt text](<assets/How to set the file path of pasted images in VSCode for Markdown/image.png>)

