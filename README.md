fetch('https://api.example.com/data')
  .then(response => {
    // Check if the request was successful
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json();  // Convert the response to JSON
  })
  .then(data => {
    console.log('Fetched data:', data);  // Use the fetched data here
  })
  .catch(error => {
    console.error('There was a problem with the fetch operation:', error);
  });
