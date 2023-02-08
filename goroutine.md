
Go language has built-in support for concurrency through goroutines and channels.

A goroutine is a lightweight thread of execution that can be created with the "go" keyword. Goroutines run concurrently with other goroutines within the same program.

Channels are a way to communicate between goroutines and synchronize their execution. A channel can be used to send and receive values between goroutines.

This approach to concurrency allows for clear and efficient management of multiple tasks without the need for locks or other synchronization mechanisms.



simple booking app in Go that uses concurrency to handle multiple bookings

```
var seats = 20
var mutex = &sync.Mutex{}

```

Here, seats variable represents the number of available seats, and mutex is a mutual exclusion lock that ensures that only one goroutine can access the seats variable at a time.

```
func book(wg *sync.WaitGroup) {
	defer wg.Done()
	mutex.Lock()
	if seats > 0 {
		seats--
		fmt.Println("Booked a seat")
	} else {
		fmt.Println("No seats available")
	}
	mutex.Unlock()
}


```


The book function takes a pointer to a sync.WaitGroup as input. The defer statement calls the Done method of the sync.WaitGroup after the book function has completed. The book function uses the Lock method of the mutex to lock access to the seats variable, ensuring that only one goroutine can access it at a time. If there are available seats, the book function decrements the number of available seats and prints "Booked a seat". If there are no available seats, it prints "No seats available". Finally, the Unlock method of the mutex is called to release the lock and allow other goroutines to access the seats variable.

```
func main() {
	var wg sync.WaitGroup

	for i := 0; i < 50; i++ {
		wg.Add(1)
		go book(&wg)
	}

	wg.Wait()
	fmt.Println("All bookings done")
}

```

The main function creates a sync.WaitGroup and launches 50 goroutines, each calling the book function. The Add method of the sync.WaitGroup is called to increment the count of the number of goroutines that need to complete. Finally, the Wait method is called to wait for all goroutines to complete before the program exits. The program prints "All bookings done" when all bookings are completed.



```
package main

import (
	"fmt"
	"sync"
)

var seats = 20
var mutex = &sync.Mutex{}

func book(wg *sync.WaitGroup) {
	defer wg.Done()
	mutex.Lock()
	if seats > 0 {
		seats--
		fmt.Println("Booked a seat")
	} else {
		fmt.Println("No seats available")
	}
	mutex.Unlock()
}

func main() {
	var wg sync.WaitGroup

	for i := 0; i < 50; i++ {
		wg.Add(1)
		go book(&wg)
	}

	wg.Wait()
	fmt.Println("All bookings done")
}

```

how you can create a REST API for the booking app using the net/http package in Go:

```
package main

import (
	"encoding/json"
	"fmt"
	"net/http"
	"sync"
)

var seats = 20
var mutex = &sync.Mutex{}

func book(wg *sync.WaitGroup) {
	defer wg.Done()
	mutex.Lock()
	if seats > 0 {
		seats--
		fmt.Println("Booked a seat")
	} else {
		fmt.Println("No seats available")
	}
	mutex.Unlock()
}

func bookSeat(w http.ResponseWriter, r *http.Request) {
	wg := &sync.WaitGroup{}
	wg.Add(1)
	go book(wg)
	wg.Wait()

	response := struct {
		Message string `json:"message"`
	}{
		Message: "Seat booked successfully",
	}

	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(response)
}

func main() {
	http.HandleFunc("/book", bookSeat)
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		fmt.Println(err)
	}
}


```

In this example, the bookSeat function handles a request to the /book endpoint. It creates a sync.WaitGroup and launches a goroutine to book a seat using the book function. The function then waits for the goroutine to complete using the Wait method. A response with a message "Seat booked successfully" is returned in JSON format.

The main function uses the http.HandleFunc method to register the bookSeat function as the handler for the /book endpoint. The http.ListenAndServe method starts the HTTP server and listens on port 8080 for incoming requests.

