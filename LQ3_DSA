// Basic structure for bhs reservation sytem
// This code handles ticket reservation for buses travelling to different location.

//Define data structurebfor buses
const buses = [
  {
    travelLocation: "Cubao",
    seats: new Array(30).fill("AVAILABLE"), // Initially, all seats are set to available
  },
  {
    travelLocation: "Baguio",
    seats: new Array(30).fill("AVAILABLE"),
  },
  {
    travelLocation: "Pasay",
    seats: new Array(30).fill("AVAILABLE"),
  },
];

// Ticket Person credentials for simplicity
const ticketPersons = [
  { username: "admin", password: "password123" }, // Ticket person account for authentication
];

// User function to authenticate ticket
function authenticateTicketPerson(username, password) {
  // Checks if provided username and password match any user in `ticketPersons`
  return ticketPersons.some(
    (user) => user.username === username && user.password === password
  );
}

// Function to display all available bus information
function displayBusInfo(buses) {
  // Loops through each bus and logs its travel location
  buses.forEach((bus, index) => {
    console.log(`Bus ${index + 1}: ${bus.travelLocation}`);
  });
}

// Function for the seat statues for a specific bus
function displayPassengerList(bus) {
  // Logs the travel location and lists all seats with their statuses
  console.log(`\n${bus.travelLocation} - Passenger List`);
  bus.seats.forEach((seat, index) => {
    console.log(`Seat ${index + 1}: ${seat}`);
  });
}

// Function to find the index of the first available seat
function findAvailableSeat(bus) {
  // Returns the index of the first "AVAILABLE" seat, or -1 if no seat is available
  return bus.seats.findIndex((seat) => seat === "AVAILABLE");
}

// Add a reservation
function addReservation(bus, seatIndex, customerName) {
  // Assigns the customer's name to the given seat if it's valid
  if (seatIndex !== -1) {
    bus.seats[seatIndex] = customerName; // Reserve the seat
    return true;
  }
  return false; // Returns false if no valid seat was found
}

// Remove a reservation
function removeReservation(bus, seatIndex) {
  // Resets a reserved seat to "AVAILABLE" if it contains a reservation
  if (bus.seats[seatIndex] !== "AVAILABLE") {
    bus.seats[seatIndex] = "AVAILABLE"; // Make the seat available again
    return true;
  }
  return false; // Returns false if no reservation exists on the seat
}

// Function to get user input (using prompt)
function getUserInput(message) {
  // Displays a prompt with the given message and returns the user's input
  return window.prompt(message);
}

// Main program loop
function startReservationSystem() {
  // Infinite loop to continuously run the reservation system
  while (true) {
    const userRole = getUserInput("Are you a TICKET PERSON or CUSTOMER?");

    // Ticket Person workflow
    if (userRole.toUpperCase() === "TICKET PERSON") {
      const username = getUserInput("Enter username:");
      const password = getUserInput("Enter password:");

      // Authenticate ticket person
      if (authenticateTicketPerson(username, password)) {
        while (true) {
          const action = getUserInput("Choose an action: LOGOUT, VIEW, MANAGE");

          if (action.toUpperCase() === "LOGOUT") {
            break; // Exit the ticket person session
          } else if (action.toUpperCase() === "VIEW") {
            // Display the passenger list for all buses
            buses.forEach((bus) => {
              displayPassengerList(bus);
            });
            getUserInput("Press Enter to continue...");
          } else if (action.toUpperCase() === "MANAGE") {
            // Manage reservations for a specific bus
            const busIndex = parseInt(getUserInput("Enter bus number (1-3):")) - 1;
            if (busIndex >= 0 && busIndex < buses.length) {
              while (true) {
                const manageAction = getUserInput("Choose an action: ADD, REMOVE, CANCEL");

                if (manageAction.toUpperCase() === "CANCEL") {
                  break; // Exit the manage mode
                } else if (manageAction.toUpperCase() === "ADD") {
                  // Add reservations for customers
                  while (true) {
                    const seatIndex = findAvailableSeat(buses[busIndex]);
                    if (seatIndex !== -1) {
                      const customerName = getUserInput("Enter customer name:");
                      if (addReservation(buses[busIndex], seatIndex, customerName)) {
                        console.log(`Reservation added successfully for ${customerName}!`);
                      } else {
                        console.log("Error adding reservation.");
                      }
                    } else {
                      console.log("Bus is full."); // No available seats
                      break;
                    }

                    if (!confirm("Add another reservation?")) {
                      break;
                    }
                  }
                } else if (manageAction.toUpperCase() === "REMOVE") {
                  // Remove a reservation by seat number
                  const seatIndex = parseInt(getUserInput("Enter seat number:")) - 1;
                  if (removeReservation(buses[busIndex], seatIndex)) {
                    console.log("Reservation removed successfully!");
                  } else {
                    console.log("Error removing reservation.");
                  }

                  if (!confirm("Remove another reservation?")) {
                    break;
                  }
                }
              }
            } else {
              console.log("Invalid bus number."); // Invalid bus selection
            }
          }
        }
      } else {
        console.log("Invalid username or password."); // Authentication failed
      }
    } else if (userRole.toUpperCase() === "CUSTOMER") {
      // Customer workflow
      displayBusInfo(buses); // Display available buses
      while (true) {
        const customerAction = getUserInput(
          "Choose an action: RESERVE, CANCEL RESERVATION, CANCEL"
        );

        if (customerAction.toUpperCase() === "CANCEL") {
          break; // Exit customer mode
        } else if (customerAction.toUpperCase() === "RESERVE") {
          // Reserve a seat on a specific bus
          const busIndex = parseInt(getUserInput("Enter bus number (1-3):")) - 1;
          if (busIndex >= 0 && busIndex < buses.length) {
            const availableSeatIndex = findAvailableSeat(buses[busIndex]);
            if (availableSeatIndex !== -1) {
              const customerName = getUserInput("Enter your name:");
              if (addReservation(buses[busIndex], availableSeatIndex, customerName)) {
                console.log("Reservation successful!");
              } else {
                console.log("Error reserving seat.");
              }
            } else {
              console.log("Bus is fully booked."); // No available seats
            }
          } else {
            console.log("Invalid bus number."); // Invalid bus selection
          }
        } else if (customerAction.toUpperCase() === "CANCEL RESERVATION") {
          // Cancel an existing reservation
          const busIndex = parseInt(getUserInput("Enter bus number (1-3):")) - 1;
          if (busIndex >= 0 && busIndex < buses.length) {
            const customerName = getUserInput("Enter your name:");
            const seatIndex = buses[busIndex].seats.findIndex(
              (name) => name === customerName
            );
            if (seatIndex !== -1) {
              if (
                confirm(
                  `Are you sure you want to cancel your reservation for seat ${
                    seatIndex + 1
                  }?`
                )
              ) {
                if (removeReservation(buses[busIndex], seatIndex)) {
                  console.log("Reservation canceled successfully!");
                } else {
                  console.log("Error canceling reservation.");
                }
              }
            } else {
              console.log("Reservation not found."); // No reservation under that name
            }
          } else {
            console.log("Invalid bus number."); // Invalid bus selection
          }
        }
      }
    } else {
      console.log("Invalid user role."); // Invalid role input
    }
  }
}

// Start the reservation system
startReservationSystem();
