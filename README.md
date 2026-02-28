
# Wayback Recon

## Banner

<img width="2389" height="1187" alt="image" src="https://github.com/user-attachments/assets/4d2b5e84-eef8-44db-9ffd-a2c0a45fdacc" />


## Description

**Wayback Recon** is a Python tool designed to collect and categorize URLs from specific domains using the [Wayback Machine](https://archive.org/web/). It leverages the Wayback Machine's CDX API to fetch historical URLs and then organizes them into predefined categories such as APIs, information leaks, file extensions, content management systems (CMS), open redirects, and Brazilian identifiers. The tool provides visually enhanced console output, thanks to the `rich` library, and saves the results in JSON and SQLite formats for further analysis.

## Features

***URL Collection:** Fetches historical URLs for a target domain using the Wayback Machine's CDX API.

***Intelligent Categorization:** Classifies collected URLs into predefined and customizable categories.

***Rich Console Output:** Presents results clearly and colorfully, highlighting relevant keywords.

***Data Export:** Saves categorized URLs to JSON files and an SQLite database.

***Category Search:** Allows searching and displaying URLs from specific categories within the SQLite database.

***Flexible Configuration:** Enables customization of categorization patterns via a `pattern_config.json` file.

***Status Code Filtering:** Ability to filter URLs based on HTTP status codes.

## Installation

**Wayback Recon** is available on [PyPI](https://pypi.org/project/wayback-recon/) and can be easily installed using `pipx` or `pip`.

### Installation via pipx (recommended)

For an isolated and managed installation, use `pipx`:

```bash

pipx install wayback-recon

```

### Installation via pip

Alternatively, you can install using `pip`:

```bash

python3.11 -m pip install wayback-recon

```

### Generating the pattern configuration file

If the `pattern_config.json` file does not exist, it will be automatically created on the first run of the tool. You can also create it manually by executing the `create_config_file()` function in the script or simply by running the tool once.

## Usage

### Creating the configuration file (if necessary)

Before using the tool, ensure that the `pattern_config.json` file exists. If not, it will be automatically generated on the first execution of the script.

### Basic Execution

To start collecting and categorizing URLs for a target domain, use the `wayback-recon` command directly (if installed via `pipx`) or `python3.11 -m wayback_recon` (if installed via `pip`):

```bash

wayback-recon -t example.com

# Or, if installed via pip:

python3.11 -m wayback-recon -t example.com

```

### Command-Line Options

*`-t`, `--target`: **(Required)** The target domain to fetch URLs for (e.g., `example.com`).

*`-p`, `--pattern-file`: Path to the custom pattern configuration JSON file (default: `pattern_config.json`).

*`-o`, `--output-file`: Custom output file name for the JSON results (default: `{domain}_wayback_recon.json`).

*`-s`, `--search`: Search and display specific categories from the SQLite database. Can be used with one or more category names (e.g., `-s apis leaks`). If used without any arguments (e.g., `wayback-recon -t example.com -s`), it will display all available categories.

*`--status-code`: Filter URLs by HTTP status codes. Provide a comma-separated list of codes (e.g., `--status-code 200,301,404`).

### Usage Examples

1.  **Fetch URLs for `example.com` and save results:**

    ```bash
    wayback-recon -t example.com
    ```

2.  **Fetch URLs and filter by status codes 200 and 301:**

    ```bash
    wayback-recon -t example.com --status-code 200,301
    ```

3.  **Fetch URLs and save to a specific output file:**

    ```bash
    wayback-recon -t example.com -o my_custom_output.json
    ```

4.  **Display available categories for `example.com`:**

    ```bash
    wayback-recon -t example.com -s
    ```

5.  **Search and display URLs in 'apis' and 'leaks' categories for `example.com`:**

    ```bash
    wayback-recon -t example.com -s apis leaks
    ```

## JSON Output Structure

The tool generates a JSON file (default: `{domain}_wayback_recon.json`) containing the categorized URLs. The structure is a dictionary where keys are the category names and values are lists of URLs found within that category. Each URL entry is a tuple containing the URL string and its HTTP status code.

Example JSON output:

```json

{

"apis": [

    ["http://example.com/api/v1/users", "200"],

    ["http://example.com/services/auth", "301"]

  ],

"leaks": [

    ["http://example.com/admin/config?apikey=AIza...", "200"]

  ],

"extensions": [

    ["http://example.com/data.json", "200"]

  ]

}

```

This JSON file can be easily parsed and used for further automated analysis or integration into other tools.

## Configuration (`pattern_config.json`)

The `pattern_config.json` file defines the keyword patterns used to categorize URLs. You can edit this file to add, remove, or modify categories and their respective keywords.

Example `pattern_config.json`:

```json

{

"apis": ["/api", "/v1", "/v2", "/services", "/rest", "/graphql", "/json"],

"leaks": ["aws", "apikey", "secret", "password", "auth", "token", "key", "access", "credential", "jwt", "kong", "kong-key", "AIza"],

"extensions": [".json", ".xml", ".txt", ".csv", ".xlsx", ".db", ".bkp", ".gz", ".tar", ".pdf", "docx"],

"cms": ["wp-", "wordpress", "joomla", "drupal", "magento", "typo3", "shopify", "prestashop"],

"open-redirect": ["/../", "?url=", "?redirect=", "?redir=", "?dest=", "?destination=","?next=", "?to=", "?rurl=", "?target=", "?site=", "?continue=", "?return=", "?go=", "?returnTo=", "?from="],

"brazilian-ids": ["cpf=", "documento=", "cnpj=", "empresa_cnpj=", "rg=", "registro=", "titulo_eleitor=", "titulo=", "cnh=", "numero_cnh=", "nis=", "pis=", "cei=", "cns=", "pis=", "pasep=", "renavam=", "cep=", "endereco_cep="],

"others": ["http"]

}

```

## Dependencies

The main dependencies for this project are:

*`requests`: For making HTTP requests to the Wayback Machine API.

*`rich`: For formatted and colored console output.

*`json`: For JSON file manipulation.

*`sqlite3`: For SQLite database management.

*`argparse`: For command-line argument parsing.

## Credits

Developed by **Renato Pinheiro** (D@ant3).

## License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/DanteArcanaelMontford/wayback-recon/blob/main/LICENSE) file for details.
