
# 📝 CUTranslateSupporter

CUTranslateSupporter is a BepInEx plugin designed for volunteer game translators and players who want to apply translation data to their game.
In addition to downloading translation files, it provides features to check translation progress, identify missing/extra keys, confirm text differences from game updates, and format the structure of your JSON files.

This might not be necessary for translators who already have their own established workflows. However, for those who find checking GitHub diffs or using external tools a bit too difficult (honestly, it was way too hard for me...), I created this tool to let you easily translate and adjust file structures entirely within the game.

TL;DR: I made this for myself.

***

## 📥 Installation

> [!TIP]
> If you have already installed **BepInEx**, please skip to **Step 2**.

### 1. ⚙️ Installing BepInEx

1. Download `BepInEx_x64_5.x.x.zip` from the [BepInEx Releases page](https://github.com/BepInEx/BepInEx/releases).
2. Extract everything EXCEPT `changelog.txt` into the same folder as the game's executable (`CasualtiesUnknown.exe`).
3. Launch the game once, wait for the title screen to appear, and then close the game.
*(This step automatically generates the necessary folders, such as `BepInEx/plugins`.)*

### 2. 🛠️ Setting up CUTranslateSupporter

1. Download `CUTranslateSupporter.zip` from [Here](https://github.com/marui-neko/CUTranslateSupporter/raw/refs/heads/main/CUTranslateSupporter.zip).
2. Extract the contents (the `BepInEx` folder) into the same folder as the game's executable (`CasualtiesUnknown.exe`).

***

## 🎮 How to Open the Menu

**Press and hold** the icon (plushie/character) at the edge of the screen in-game to open the menu.
(*Note: If you have set `devmode=false` in the settings, holding the button will trigger the automatic download instead.*)

***

## ✨ Features & Usage

The menu is divided into two main tabs.
Detailed instructions are provided in the tooltips in-game, but here is a quick overview.

### 📁 Tab 1: Translation Download

Basic features primarily for players who just want to download and play with translation data.

* **Download Buttons**: Download translation data from specific lists or URLs.
* **Settings**: Change the menu icon's appearance (plushie/character) and toggle the auto-wrap feature (only available if CUFontPatcher is installed).

***

### 🛠️ Tab 2: JSON Diff

Advanced features primarily for translators and developers. It helps with comparing JSON files and merging updates.

#### 1. 🔍 Compare

Compares your current translation data with the latest `EN.json` to check your translation progress.

* **Left Click (Short)**: Displays the translation progress (%), missing keys, extra keys, and a list of untranslated items on the screen.
* **Hold Left Click for 1s**: Generates a full comparison log (text file) and **automatically opens the save folder**.
* **Hold Right Click for 1s**: Generates a **CSV file containing only untranslated items**. You can edit this CSV in a spreadsheet and easily apply the translations using the "CSV Import" feature mentioned below.

#### 2. 🔄 Update Sync (3-way merge)

Use this feature when English texts are added or modified due to game updates.
It compares three files: "Old EN", "Latest EN", and "Current Translation Data". It safely inserts only the changed English texts while preserving your already translated parts.

After processing, it outputs a log of newly added or modified keys, and automatically runs "Sync Structure" (explained below). Keys with modified original texts will be overwritten with the newest English text, but your previous translation will be kept in the log as a backup for you to review and re-translate.

* **Preparation**: You must place the old version of `en.json` (the one your translation was based on) into the `CasualtiesUnknown_Data\Lang\BaseEN` folder. (*The BaseEN folder is automatically created when you click the button.*)
* **Left Click (Click 5 times to prevent accidental execution)**: Executes the update difference merge process.

##### 2.5. 📑 CSV Import Feature

Accessible from the Update Sync button.
It overwrites your current translation file with the translated data from your CSV file in bulk.

* **Preparation**: Place a CSV file with **the exact same name as your current translation file** (e.g., `ja-JP.csv` for `ja-JP.json`) into the `CasualtiesUnknown_Data\Lang\BaseEN` folder.
* **Hold Right Click for 1s**: Reads the data in the "Translation" column corresponding to the "Key" in the CSV and applies it.

#### 3. ✨ Sync Structure (2-way merge)

A feature that perfectly aligns the format of your translation JSON file (line breaks, spacing, key order) with the official `EN.json`.
Use this to clean up your data if the visual formatting gets messed up after editing in a text editor.

- **Left Click (Click 5 times to prevent accidental execution)**: Executes the format synchronization.
- **Important Note**: This feature is purely for matching the "structure (appearance)". It does NOT check for changes in the original text itself. Always use "Update Sync" if your translation data structure does not match the latest EN.json, or if you need to apply text updates. (If Update Sync is successful, this format sync is performed automatically anyway.)

***

## ⚠️ Important Warnings

* "Update Sync", "CSV Import", and "Sync Structure" in Tab 2 are powerful tools that **directly overwrite your translation files**.
* Just in case of unexpected bugs or data loss, **PLEASE ALWAYS MAKE A BACKUP of your translation files before executing them!**

***

## ⚙️ Guide to setting.txt

You can customize the mod to fit your playstyle or translation workflow by editing `setting.txt` located in `BepInEx\plugins\CUTranslateSupporter`.
Details are also written inside the file, but here is a more in-depth explanation.

```ini
# ==========================================
# CU Translate Supporter Settings
# ==========================================

```ini
Target Language=
```
**[Target Language]**
Specify the translation file name you want to apply automatically when the game starts (e.g., `ja-JP.json` , `ES.json`).
If this file does not exist in the Lang folder, **the mod will automatically download it from the official repository on your first launch and apply the language settings for you.** This saves players a lot of manual setup time.

```ini
github_url=https://raw.githubusercontent.com/marui-neko/scavgame-locale/japanese-translation/ja-JP.json, https://...
```
**[Workspace Repository URLs]**
Specify the raw links to the repository where you are currently working, or where the latest unmerged translation data is hosted. You can register multiple URLs separated by commas (`,`) or spaces.
The URLs set here are used by the "Download Latest Translation from Workspace" button in the UI. The mod will automatically find and download the URL that matches your currently selected language file name.

```ini
github_api_url=https://api.github.com/repos/Orsoniks/scavgame-locale/contents
```
**[Official Repository API]**
The API URL for the official translation repository. Basically, **do not change this.**

```ini
compare_ignore_key= lrd, epda, firmware, wiki, experiment, Milky, Dune, fullmouthchars, underwaterchars, Shelly, 
```
**[Keys to Ignore in Compare]**
Specify keys that you **do not want to be counted as untranslated** in the "Compare" feature.
Registering proper nouns, system data that don't need translation, or "massive text blocks you haven't started translating yet" here helps you measure your actual translation progress (%) more accurately.

```ini
compare_ignore_text=AAAAAAAA!|AAAAAAAAAAAAAA!|Si tir ti vucot svaklar si mi.|Kovgam varmunch persvek sia narod!|
```
**[Texts to Ignore in Compare]**
Instead of keys, you can specify **"exact text strings"** to exclude from the untranslated count in the "Compare" feature. Separate multiple texts with a pipe `|`.
This is extremely useful for specifying lines that are just "screams" mixed into character dialogues/notes arrays, or int 0 languages that should remain as they are.

***

## 🛠️ Behind the Scenes (Internal Logic)

This tool doesn't just blindly overwrite JSON files. It uses several approaches to maintain key integrity and formatting. Here is a brief explanation of how each feature works internally.

### 1. How 'Compare' Works
It recursively extracts multi-layered JSON objects and arrays into flat strings with their hierarchy paths (e.g., `Root.SomeArray[0].TextKey`), and performs set operations (like set differences) between `EN.json` and your translation data.
This allows it to accurately pinpoint "which keys are missing or extra" no matter how deep the hierarchy is.

```csharp
// Part of the logic to recursively get paths
private void GetKeysRecursively(JToken token, string currentPath, List<string> keys)
{
    if (token is JObject obj)
    {
        foreach (var prop in obj.Properties())
        {
            string newPath = string.IsNullOrEmpty(currentPath) ? prop.Name : currentPath + "." + prop.Name;
            if (prop.Value.Type != JTokenType.Array) keys.Add(newPath);
            GetKeysRecursively(prop.Value, newPath, keys);
        }
    }
    // Skipping array processing because it's too long!
}
```

### 2. How 'Sync Structure' (2-way merge) Works
If you serialize using standard JSON libraries (like Newtonsoft.Json), the unique indentation and line break quirks of the original file are often lost.
Therefore, this feature uses a text-based process: **"Read `EN.json` line by line as a text file, and replace only the value parts with translated texts."** This allows you to pour your translation data in without breaking the official formatting.

```csharp
// A glimpse of the line-by-line replacement logic
int lineIndex = lineInfo.LineNumber - 1;
string escapedOldText = JsonConvert.SerializeObject(oldText);
string escapedEnText = JsonConvert.SerializeObject(enText);

if (lineIndex >= 0 && lineIndex < baseJsonLines.Length)
{
    string line = baseJsonLines[lineIndex];
    int lastIdx = line.LastIndexOf(escapedEnText); // Search and replace the value from the back of the line
    if (lastIdx >= 0)
    {
        baseJsonLines[lineIndex] = line.Remove(lastIdx, escapedEnText.Length)
                                       .Insert(lastIdx, escapedOldText);
    }
}
```

### 3. How 'Update Sync' (3-way merge) Works
When writing a JSON sync script, the most troublesome part is **"data where multiple lines (arrays) are stored under a single key."**
To prevent this from breaking, a 2-way comparison between "Current EN" and "Translation Data" wasn't enough; a 3-way merge based on the "Old EN" (which the translation was based on) was essential.
It simultaneously traverses three JSON trees: "Old Original", "New Original", and "Current Translation Data", deciding the merge result by looking at the state of each node.
- **String Evaluation**: Compares the Old and New Original. If there's a difference, it assumes the original text was updated and adopts the New Original. If there's no change, it keeps your translation.
- **Array Difference Extraction (LCS Algorithm)**:
For arrays where the number of elements has changed, it calculates the difference using LCS (Longest Common Subsequence). However, to prevent freezes and false matching due to computational complexity (O(N×M)), it optimizes by **"trimming the perfectly matching parts at the beginning and end"** from the calculation beforehand. Thanks to this, even if massive amounts of epda or notes are added in the future, it should quickly and accurately calculate the differences and insert new texts in the correct positions.

```csharp
// String node evaluation logic in 3-way merge
string oldStr = oldEn.ToString();
string newStr = newEn.ToString();
string tgtStr = target.ToString();

if (oldStr != newStr) 
{
    // If original text changed, overwrite with new English
    modifiedKeys.Add(new ModifiedKeyInfo { Path = currentPath, OldText = oldStr, NewText = newStr, TargetText = tgtStr });
    return newEn.DeepClone();
}
else 
{
    // If original text is unchanged, keep current translation
    return target.DeepClone();
}
```
```csharp
// Prefix/suffix trimming optimization in array merge (just a part of it!)
int prefixCount = 0;
while (prefixCount < n && prefixCount < m && TokensAreEqual(oldArr[prefixCount], newArr[prefixCount])) {
    prefixCount++; // Skip matching parts at the beginning
}

int suffixCount = 0;
while (suffixCount < (n - prefixCount) && suffixCount < (m - prefixCount) && 
       TokensAreEqual(oldArr[n - 1 - suffixCount], newArr[m - 1 - suffixCount])) {
    suffixCount++; // Skip matching parts at the end
}

// Pass only the non-matching middle parts to the LCS DP table
int lcsOldCount = n - prefixCount - suffixCount;
int lcsNewCount = m - prefixCount - suffixCount;
```

I'm just a dummy... I can only write these incredibly long, clunky codes... 
Please, someone make a simpler automation script for meeeeee...!!!
