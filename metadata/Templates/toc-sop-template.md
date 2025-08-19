<%*
  // 1. Get the full vault-relative path of the current note, e.g.
  //    "Application/Digipos/Application Overview.md"
  const fullPath = tp.file.path(true);

  // 2. Extract the folder by chopping off everything after the last "/"
  const lastSlash = fullPath.lastIndexOf("/");
  const baseFolder = lastSlash === -1
    ? ""                                    // note is in vault root
    : fullPath.slice(0, lastSlash);        // e.g. "Application/Digipos"

  // 3. Build the SOP folder path
  const sopFolder = baseFolder
    ? `${baseFolder}/SOP`                  // "Application/Digipos/SOP"
    : "SOP";                               // fallback if note in root

  // 4. Debug output—verify it matches your vault sidebar
  tR += `> Debug: looking in \`${sopFolder}\`\n\n`;

  // 5. Grab all markdown files under that folder
  const sopFiles = app.vault.getMarkdownFiles()
    .filter(f => f.path.startsWith(sopFolder + "/"))
    .sort((a, b) => a.basename.localeCompare(b.basename));

  // 6. Render the list or “not found” message
  if (!sopFiles.length) {
    tR += `*No SOP files found in \`${sopFolder}\`*`;
  } else {
    tR += `# SOP Contents\n\n`;
    for (const file of sopFiles) {
      const link = app.metadataCache.fileToLinktext(file, tp.file.path(true));
      tR += `- [[${link}]]\n`;
    }
  }
%>