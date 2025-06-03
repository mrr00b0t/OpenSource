# Digital Product Showcase & Content Manager

A simple, modern, and customizable static website template for showcasing and selling digital products. The website content is dynamically loaded from a `data.json` file, which can be easily managed using a Python-based CLI content manager.

## Features

### Website (`index.html`, `js/script.js`, `css/style.css`)
* **Dynamic Product Display:** Products are loaded from `data.json` and displayed in a clean grid.
* **Product Details & Plans:** Users can view detailed descriptions and various pricing plans for each product.
* **Multi-Currency Display:** Supports showing prices in USD and a secondary currency (e.g., MMKS), configured in `data.json`.
* **Out of Stock Visuals:** Clearly indicates if a product or a specific plan is out of stock.
* **Multi-Step Purchase Flow:** Guides users from product selection to plan choice, payment method selection, and payment instructions.
* **Dynamic Payment Instructions:** Shows relevant payment details based on the selected method.
* **Receipt Page:** Displays a summary of the selected product and plan, with a button to contact via Telegram for order confirmation.
* **Dynamic Admin Note:** Displays an admin-configurable note on the homepage (visibility and content managed via `data.json`).
* **Dynamic FAQ Section:** Displays a list of frequently asked questions and answers, managed via `data.json`.
* **Theme Switcher:** Light and Dark mode support with preference saved in local storage.
* **Responsive Design:** Aims for good usability on different screen sizes (basic responsiveness from CSS).
* **Hardcoded Footer Links:** Footer includes fixed links for "Contact Developer" and "Create Your Own Website".

### Content Manager (`content_manager.py`)
* **CLI Interface:** Easy-to-use command-line tool to manage website content without directly editing JSON.
* **Manages `data.json`:** Loads from and saves changes to the `data.json` file.
* **Automatic `data.json` Initialization:** Creates a default `data.json` structure if the file is missing.
* **Site Configuration:** Edit persistent header messages and Telegram links used on the site.
* **Product Management:**
    * Add, edit, and remove products.
    * View product details.
    * Toggle "Out of Stock" status for entire products.
* **Plan Management (Nested within Products):**
    * Add, edit, and remove pricing plans for each product.
    * Set plan-specific details (name, price in USD & MMKS, duration).
    * Toggle "Out of Stock" status for individual plans.
* **Admin Note Management:**
    * Edit the title and content (multi-paragraph) of the admin note.
    * Toggle visibility of the admin note on the website.
* **FAQ Management:**
    * Add, edit, and remove FAQ items (question and answer pairs).
* **Payment Method Management:**
    * Edit details of existing payment methods (e.g., KPay account info, Binance UID, notes).

## Tech Stack
* **Frontend:** HTML5, CSS3, Vanilla JavaScript (ES6+)
* **Content Management Script:** Python 3
* **Data Storage:** JSON (`data.json`)

## File Structure
├── css/
│   └── style.css         # Main stylesheet
├── js/
│   └── script.js         # Main JavaScript for website logic
├── data.json             # Stores all website content (products, FAQs, etc.)
├── content_manager.py    # Python script for managing data.json
├── index.html            # Main HTML file for the website
└── README.md             # This file


## Setup and Installation

### Prerequisites
* A modern web browser (e.g., Chrome, Firefox, Edge).
* Python 3 installed on your system (to use the `content_manager.py` script).
* A local web server for running `index.html` (required because the website uses the `fetch` API to load `data.json`, which is subject to CORS policy when run directly from `file:///`).

### 1. Get the Project Files
Clone this repository or download the ZIP file and extract it to your local machine.

### 2. Running the Website
Due to browser security restrictions (CORS policy for `fetch` API with local files), you need to serve `index.html` through a local web server.

A simple way to do this if you have Python installed:
1.  Open your terminal or command prompt.
2.  Navigate to the root directory of the project (where `index.html` is located).
3.  Run the command:
    * For Python 3: `python -m http.server`
    * For Python 2 (if you have it): `python -m SimpleHTTPServer`
4.  The server will usually start on port 8000. Open your web browser and go to: `http://localhost:8000`

Alternatively, you can use other local server tools like VS Code's "Live Server" extension.

### 3. Using the Python Content Manager
The `content_manager.py` script allows you to edit the website's content stored in `data.json`.
1.  Ensure Python 3 is installed.
2.  Open your terminal or command prompt.
3.  Navigate to the root directory of the project.
4.  Run the script using: `python content_manager.py`
5.  Follow the on-screen menus to manage products, site configuration, FAQs, etc.
    * **Note:** If you encounter issues saving `data.json` (especially on Windows or if the script was run without sufficient privileges previously), you might need to run the script with administrator privileges (e.g., "Run as administrator" for `cmd.exe` or PowerShell on Windows, or `sudo python content_manager.py` on Linux/macOS). This is often due to file permissions.

## Usage Guide

### Managing Content
You have two primary ways to manage the website's content:
1.  **Directly Editing `data.json`:**
    You can open `data.json` in a text editor and modify its content directly. The structure includes:
    * `siteConfig`: General site settings like the persistent header message and Telegram links.
        * `telegramBotLink`: **Crucial for the receipt page's "Go to Telegram" button.** Ensure this is a correct, full Telegram URL (e.g., `https://t.me/YourTelegramUsernameOrBot`).
    * `products`: An array of product objects, each with an `id`, `name`, `description`, `isOutOfStock` status, and an array of `plans`.
        * `plans`: Each plan has an `id`, `name`, `price` (USD), `mmksPrice`, `duration`, and `isOutOfStock` status.
    * `paymentMethods`: An array of payment methods, each with an `id`, `name`, and an `instructions` object (e.g., `accountName`, `accountNumber`, `notes` for KPay; `uid`, `currency`, `notes` for Binance).
    * `adminNote`: An object with `title`, `isVisible` (boolean), and `content` (an array of strings, where each string is a paragraph).
    * `faq`: An array of FAQ objects, each with an `id`, `question`, and `answer`.

2.  **Using `content_manager.py` (Recommended):**
    This script provides a user-friendly CLI to manage all sections of `data.json`:
    * **Main Menu Options:**
        * `Manage Products`: Add, edit, view details, remove products. Includes sub-management for product plans (add, edit, remove, toggle stock). You can also toggle the overall stock status of a product.
        * `Edit Site Configuration`: Change the persistent header message and key Telegram links used by the site. **Prompts will guide you to enter full URLs for Telegram links.**
        * `Edit Admin Note`: Modify the title, paragraphs of content, and visibility of the admin note.
        * `Manage FAQs`: Add, edit, or remove FAQ entries.
        * `Manage Payment Methods`: Edit the details (name, account info, notes) for existing payment methods like KPay and Binance.
    * The script handles ID generation for new items where applicable and provides clear prompts for editing.

### Website Flow
1.  **Homepage (`index.html`):** Displays the admin note (if visible), the list of products, and the FAQ section.
2.  **Product Interaction:** Clicking on an available product takes the user to product options.
3.  **Product Options:** User can choose to view product details or see available plans.
4.  **Plan Selection:** User selects a pricing plan for the chosen product.
5.  **Payment Method Selection:** User chooses a payment method.
6.  **Payment Instructions:** Detailed instructions for the selected payment method are shown, along with the total amount.
7.  **Receipt Page:** After the user indicates they've completed payment, a receipt summary is shown with a "Go to Telegram" button for order confirmation. This button uses the `telegramBotLink` from `data.json`.

## Customization
* **Styling:** Modify `css/style.css` to change the visual appearance of the website.
* **Content Structure:** While `data.json` can be extended, be aware that adding new top-level keys or significantly changing the structure of existing items (like products or plans) would require corresponding updates to `js/script.js` to display the new data and potentially to `content_manager.py` to manage it.

## Video Tutorial
Watch the comprehensive video guide on setting up and using this project:
[Project Tutorial on YouTube](https://youtu.be/f3gEA14XUK8)

## Contributing
Contributions are welcome! If you have suggestions or improvements, feel free to:
1.  Fork the repository.
2.  Create a new branch (`git checkout -b feature/AmazingFeature`).
3.  Commit your changes (`git commit -m 'Add some AmazingFeature'`).
4.  Push to the branch (`git push origin feature/AmazingFeature`).
5.  Open a Pull Request.

## License
This project is open source. Please choose and add a license file if you wish (e.g., MIT License is a popular choice).

## Author & Contact
* **GitHub:** [mrr00b0t](https://github.com/mrr00b0t)
* **Telegram (for development contact/queries regarding this project):** [Tyrell_TheForgottenOne](https://t.me/Tyrell_TheForgottenOne)
