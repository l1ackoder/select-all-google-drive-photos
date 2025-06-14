# Google Photos Auto-Selector (No Image Load)
==For performence boost block the image load on the site==
- go to site setting
- on the image option, select block image

This script automates the selection of all photos in [Google Photos](https://photos.google.com/) without deleting them, and without requiring images to load.

Ideal for large photo libraries where performance is important — especially when image loading is blocked via browser settings.

## 🔧 Features

- ✅ Selects all photos automatically
- 🚫 No image loading required (works even with images disabled)
- 🔁 Scrolls through your entire photo library automatically
- 🧠 Stops automatically when all visible photos are selected

## 📦 Usage

1. **Open Google Photos** in your browser: [https://photos.google.com](https://photos.google.com)
2. (Optional) **Block image loading** in your browser to speed up the script
3. **Open DevTools** (`F12` or right-click → Inspect)
4. Go to the **Console** tab
5. Paste the following script:

```javascript
const checkboxSelector = '.ckGgle[aria-checked=false]';
const photoDivSelector = ".yDSiEe.uGCjIb.zcLWac.eejsDc.TWmIyd";

async function selectAllPhotosWithoutImages() {
    const wait = (ms) => new Promise(resolve => setTimeout(resolve, ms));

    const getUncheckedCheckboxes = () => [...document.querySelectorAll(checkboxSelector)];

    const scrollPhotoListBy = (pixels) => {
        const photoDiv = document.querySelector(photoDivSelector);
        photoDiv.scrollBy(0, pixels);
    };

    let previousCheckboxCount = 0;
    let unchangedScrollCount = 0;
    const maxUnchangedScrolls = 10;

    while (unchangedScrollCount < maxUnchangedScrolls) {
        const checkboxes = getUncheckedCheckboxes();
        for (const checkbox of checkboxes) {
            checkbox.click();
            await wait(5);
        }

        scrollPhotoListBy(1000);
        await wait(300);

        const currentCount = getUncheckedCheckboxes().length;
        if (currentCount === previousCheckboxCount) {
            unchangedScrollCount++;
        } else {
            unchangedScrollCount = 0;
            previousCheckboxCount = currentCount;
        }

        console.log(`Remaining checkboxes: ${currentCount}`);
    }

    console.log("Finished selecting all photos.");
}

await selectAllPhotosWithoutImages();
````

6. Hit **Enter**, and let it run!

## 🛑 Notes

* This script **does not delete** any photos.
* To delete selected photos manually, use the delete button after the script completes.
* It is designed for **personal use only**.

