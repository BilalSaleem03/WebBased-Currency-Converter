# WebBased-Currency-Converter
# Currency Converter README

## Description

This project is a simple currency converter web application. It allows users to select two currencies, enter an amount, and convert that amount from one currency to the other. The application uses the ExchangeRate-API to fetch the latest exchange rates.

## Features

- Select currencies from dropdown lists.
- Displays flags for selected currencies.
- Fetches real-time exchange rates from ExchangeRate-API.
- Converts the specified amount and displays the result.

## Files

- `index.html`: Contains the structure of the web page.
- `styles.css`: Contains the styling for the web page.
- `script.js`: Contains the JavaScript code to handle the currency conversion and update the UI.

## Getting Started

### Prerequisites

To run this project, you'll need:

- A modern web browser
- An internet connection to fetch exchange rates

### Installation

1. Clone the repository to your local machine.
2. Open the `index.html` file in your web browser.

### Usage

1. Open the web page in your browser.
2. Select the currencies you want to convert from and to from the dropdown lists.
3. Enter the amount you want to convert.
4. Click the "Convert" button.
5. The converted amount will be displayed on the screen.

### JavaScript Code Explanation

#### Currency List

The `countryList` object contains currency codes as keys and their respective country codes as values. This is used to map currency codes to their corresponding country flags.

```javascript
const countryList = {
    AED: "AE",
    AFN: "AF",
    ...
};
```

#### Elements Selection

The script selects various elements from the DOM for manipulation.

```javascript
let baseURL = "https://v6.exchangerate-api.com/v6/78d1514be026b2e116fa23a2/latest/";
let dropDowns = document.querySelectorAll(".form-inner #contry");
const btn = document.querySelector("button");
let fromCountry = document.querySelector(".from-country select");
let toCountry = document.querySelector(".to-country select");
let message = document.querySelector(".message");
```

#### Populate Dropdowns

The script populates the dropdown lists with currency codes and sets default selections for "USD" and "PKR".

```javascript
for (let select of dropDowns) {
    for (let currCode in countryList) {
        let option = document.createElement("option");
        option.innerText = currCode;
        option.value = currCode;
        if (select.name === "from" && currCode === "USD") {
            option.selected = "selected";
        } else if (select.name === "to" && currCode === "PKR") {
            option.selected = "selected";
        }
        select.append(option);
    }
    select.addEventListener("change", (event) => {
        upgrateFlag(event.target);
    });
}
```

#### Update Flags

The script updates the flag images based on the selected currency.

```javascript
let upgrateFlag = (element) => {
    let currCode = element.value;
    let countryCode = countryList[currCode];
    let newSrc = "https://flagsapi.com/" + countryCode + "/flat/64.png";
    let img = element.parentElement.querySelector("img");
    img.src = newSrc;
};
```

#### Convert Currency

The script handles the conversion when the "Convert" button is clicked. It fetches the exchange rate and calculates the converted amount.

```javascript
btn.addEventListener("click", async () => {
    let amount = document.querySelector("input");
    if (amount.value === "" || amount.value <= 0) {
        amount.value = "1";
    }
    const URL = baseURL + fromCountry.value.toLowerCase();
    let data = await fetch(URL);
    let data2 = await data.json();
    let conversionRate = data2.conversion_rates[toCountry.value];
    let result = amount.value * conversionRate;
    message.innerText = amount.value + " " + fromCountry.value + " = " + result + " " + toCountry.value;
});
```

## API

The application uses the ExchangeRate-API to fetch the latest exchange rates. You need an API key to use this service. Replace the placeholder API key in the `baseURL` variable with your own API key.

```javascript
let baseURL = "https://v6.exchangerate-api.com/v6/YOUR_API_KEY/latest/";
```

## Acknowledgements

- [ExchangeRate-API](https://www.exchangerate-api.com/) for providing the exchange rate data.
