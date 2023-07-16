const fs = require('fs');
const readline = require('readline');

// Function to read CSV files
function readCSVFile(filePath) {
  const fileData = fs.readFileSync(filePath, 'utf8');
  const rows = fileData.split('\n');
  const headers = rows[0].split(',');
  const data = [];

  for (let i = 1; i < rows.length; i++) {
    const values = rows[i].split(',');
    const row = {};

    for (let j = 0; j < headers.length; j++) {
      row[headers[j]] = values[j];
    }

    data.push(row);
  }

  return data;
}

// Read CSV files
const productData = readCSVFile('product_detail.csv');
const refrigeratorData = readCSVFile('refrigerator_detail.csv');
const offerData = readCSVFile('special_offer.csv');
const priceData = readCSVFile('final_price.csv');
const serviceData = readCSVFile('service_sheet.csv');
const dealerData = readCSVFile('dealer_detail.csv');

// Function to get available locations
function getAvailableLocations() {
  const cities = dealerData.map(dealer => dealer.City);
  const uniqueCities = [...new Set(cities)];
  return uniqueCities;
}

// Function to find nearest stores based on location
function findNearestStores(location) {
  const nearestStores = dealerData.filter(dealer => dealer.City.toLowerCase() === location.toLowerCase());
  return nearestStores;
}

// Function to sort dealers by ratings
function sortDealersByRating(dealers) {
  const sortedDealers = dealers.sort((a, b) => {
    if (a.Ratings === b.Ratings) {
      // If ratings are the same, sort by dealer name
      return a['Dealers Name'].localeCompare(b['Dealers Name']);
    }
    return b.Ratings - a.Ratings;
  });
  return sortedDealers;
}

// Function to display contact details of a dealer
function displayContactDetails(dealer) {
  console.log(`Dealer Name: ${dealer['Dealers Name']}`);
  console.log(`Contact Number: ${dealer['Contact Number']}`);
  console.log(`Product: ${dealer['Availability']} available`);
  console.log('------------------');
}

// Function to process user input and generate chatbot response
function getChatbotResponse(userInput) {
  const inputRegex = /^\d$/;

  if (inputRegex.test(userInput)) {
    const locationIndex = parseInt(userInput) - 1;

    const availableLocations = getAvailableLocations();

    if (locationIndex >= 0 && locationIndex < availableLocations.length) {
      const location = availableLocations[locationIndex];

      // Find nearest stores based on location
      const nearestStores = findNearestStores(location);

      if (nearestStores.length > 0) {
        // Sort stores by ratings
        const sortedDealers = sortDealersByRating(nearestStores);

        // Get the highest rating among the nearest stores
        const highestRating = sortedDealers[0].Ratings;

        // Filter stores with the highest rating
        const storesWithHighestRating = sortedDealers.filter(
          dealer => dealer.Ratings === highestRating
        );

        // Display contact details of the dealers with the highest rating
        console.log(`These are the nearest stores based on the provided location (${location}). Contact details of the top-rated store(s) with a rating of ${highestRating}:`);
        storesWithHighestRating.forEach(dealer => {
          displayContactDetails(dealer);
        });

        // Ask if the user wants to purchase online
        console.log('Would you like topurchase online? (yes/no)');
        return 'location-confirmation';
      } else {
        return 'No nearest stores found for the provided location.';
      }
    } else {
      return 'Invalid location number. Please choose a valid location number.';
    }
  } else if (userInput.toLowerCase() === 'yes' || userInput.toLowerCase() === 'no') {
    return 'Invalid input. Please enter a valid location number.';
  } else {
    return 'Invalid input. Please enter a valid location number or "yes" or "no".';
  }
}

// Function to handle online purchase
function handleOnlinePurchase(userInput) {
  if (userInput.toLowerCase() === 'yes') {
    // Display refrigerator details
    console.log('Here are some available refrigerator models:');
    
    refrigeratorData.forEach((refrigerator, index) => {
      console.log(`${index + 1}. ${refrigerator['Company']} ${refrigerator['Model Name']} - Price: ${refrigerator['Price']}`);
    });
    console.log('Please choose a refrigerator model (enter the number).');
    handleOnlinePurchaseSelection('refrigerator');
  } else if (userInput.toLowerCase() === 'no') {
    // Display product categories
    console.log('Available product categories:');
    productData.forEach((product) => {
      console.log(`- ${product['Product Name']}`);
    });
    console.log('Please choose a product category (enter the category name).');
    handleOnlinePurchaseSelection('product');
  } else {
    console.log('Invalid input. Please enter "yes" or "no".');
    processUserInput();
  }
}

// Function to handle online purchase selection
function handleOnlinePurchaseSelection(selectionType) {
  const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout,
  });

  rl.question('User: ', (userInput) => {
    rl.close();

    const firstCharacter = userInput.trim().charAt(0);

    if (selectionType === 'refrigerator') {
      const selectionIndex = parseInt(firstCharacter) - 1;

      if (isNaN(selectionIndex) || selectionIndex < 0 || selectionIndex >= refrigeratorData.length) {
        console.log('Invalid selection. Please choose a valid refrigerator model number.');
        handleOnlinePurchaseSelection('refrigerator');
        return;
      }

      const selectedRefrigerator = refrigeratorData[selectionIndex];
      console.log(`You have selected ${selectedRefrigerator['Company']} ${selectedRefrigerator['Model Name']}.`);

      // Suggest payment gateway options with discounts
      suggestPaymentGateways();
    } else if (selectionType === 'product') {
      // Handle product category selection
      const selectedCategory = userInput.trim();

      const categoryExists = productData.some((product) => {
        return product['Product Name'].toLowerCase() === selectedCategory.toLowerCase();
      });

      if (!categoryExists) {
        console.log('Invalid category. Please choose a valid product category.');
        handleOnlinePurchaseSelection('product');
        return;
      }

      console.log(`You have selected ${selectedCategory} category.`);

      // Suggest payment gateway options with discounts
      suggestPaymentGateways();
    }
  });
}
// Function to suggest payment gateway options with discounts
// Function to suggest payment gateway options with discounts
function suggestPaymentGateways() {
  // Placeholder payment gateways data
  const paymentGateways = [
    {
      name: 'Gateway A',
      discount: 10,
    },
    {
      name: 'Gateway B',
      discount: 15,
    },
    {
      name: 'Gateway C',
      discount: 20,
    },
  ];

  // Display payment gateway options with discounts
  paymentGateways.forEach((gateway) => {
    console.log(`${gateway.name} - ${gateway.discount}% discount`);
  });

  // Add your logic to suggest payment gateways with actual discounts
}

// Function to process user input
function processUserInput() {
  const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout,
  });

  rl.question('User: ', (userInput) => {
    const response = getChatbotResponse(userInput);
    console.log('Chatbot: ' + response);

    if (response === 'location-confirmation') {
      rl.question('User: ', (userInput) => {
        handleOnlinePurchase(userInput);
        
      });
    } else {
      rl.close();
    }
  });
}

// Start the conversation
console.log('Welcome! Please choose a location number to find the nearest stores:');
const availableLocations = getAvailableLocations();
availableLocations.forEach((location, index) => {
  console.log(`${index + 1}. ${location}`);
});


// Process user input
processUserInput();
