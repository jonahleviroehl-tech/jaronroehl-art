# Add New Artwork

Scan the project for new artwork images and automatically update the site.

## Workflow

### 1. Detect New Artwork Folders

Scan `public/images/` for folders that do NOT have a matching entry in `src/data/artworks.json` (match by `folder` field).

List any new folders found. If none, tell the user "No new artwork folders found. Drop a folder of images into public/images/ first."

### 2. Auto-Detect Main vs Sub Pictures

For each new folder:
- **Main picture** = the image with the LARGEST file size (full painting shot is typically highest resolution)
- **Sub pictures** = all remaining images, sorted by file size descending
- Supported formats: .jpg, .jpeg, .png, .JPG, .JPEG, .PNG

### 3. Ask the User for Metadata

For each new artwork, ask the user using AskUserQuestion:
- **Category**: "Ölgemälde" or "Zeichnungen" (default: Ölgemälde)
- **Medium**: e.g. "Öl auf Leinwand", "Bleistift auf Papier" (default: Öl auf Leinwand)
- **Dimensions**: e.g. "90 x 130 cm"
- **Featured**: Show on homepage? (default: true)

Generate the `id` automatically from the folder name (lowercase, spaces to hyphens).

### 4. Update artworks.json

Read `src/data/artworks.json`, append the new artwork entry with this structure:
```json
{
  "id": "<generated-id>",
  "category": "<user-input>",
  "medium": "<user-input>",
  "dimensions": "<user-input>",
  "folder": "<folder-name>",
  "images": ["<main-image>", "<sub1>", "<sub2>", ...],
  "description": "",
  "featured": <true/false>,
  "hero": false
}
```

Main image is ALWAYS first in the `images` array.

### 5. Verify Build

Run `npx astro build` to verify everything compiles correctly.

### 6. Deploy

Stage the new image files and the updated artworks.json:
```
git add public/images/<FolderName>/ src/data/artworks.json
git commit -m "Add artwork: <folder-name>"
git push origin main
```

This triggers the GitHub Actions deployment automatically.

### 7. Confirm

Tell the user the artwork has been added and the site will be live in ~2 minutes after GitHub Actions completes.
