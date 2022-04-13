# Go Goroutine select

## select
Syntax to realize value passing through channels from multiple goroutines.  
Without using select, the state of waiting to receive a value would continue, but select allows pinpoint passing of the value from the target channel.  

## Code
```Go
// Function to find one prime number greater than or equal to a certain number
func primeRoutine(targetLine int, primeChan chan int) {
	fmt.Printf("%d Line Target Goroutine Wake UP \n", targetLine)
	i := 0
	for {
		// Increment value
		i++
		// Prime number check
		if checkPrime(i) && targetLine < i {
			fmt.Printf("%d Line Target Goroutine Shut Down \n", targetLine)
			// Send data to channel
			primeChan <- i
			return
		}
	}
}

// main function
func main() {

	// Base value of prime number to be sought
	targetLine1 := 100000
	targetLine2 := 150000
	targetLine3 := 200000
	targetLine4 := 250000
	targetLine5 := 300000

	// Channel Creation
	primeChan1 := make(chan int)
	primeChan2 := make(chan int)
	primeChan3 := make(chan int)
	primeChan4 := make(chan int)
	primeChan5 := make(chan int)

	// Start multiple Goroutines to find prime numbers
	go primeRoutine(targetLine1, primeChan1)
	go primeRoutine(targetLine2, primeChan2)
	go primeRoutine(targetLine3, primeChan3)
	go primeRoutine(targetLine4, primeChan4)
	go primeRoutine(targetLine5, primeChan5)

	loop:
		// Waiting for a response from Goroutine using select
		for {
			select {

			case primeResult1 := <-primeChan1:
				fmt.Printf("Answer1 : %d \n", primeResult1)

			case primeResult2 := <-primeChan2:
				fmt.Printf("Answer2 : %d \n", primeResult2)

			case primeResult3 := <-primeChan3:
				fmt.Printf("Answer3 : %d \n", primeResult3)

			case primeResult4 := <-primeChan4:
				fmt.Printf("Answer4 : %d \n", primeResult4)

			case primeResult5 := <-primeChan5:
				fmt.Printf("Answer5 : %d \n", primeResult5)
				// When the fifth one is received, exit the for loop
				break loop

			default:
		}
	}
}
```

## Output Sample
~ $ go build -o main main.go  
~ $ ./main  
100000 Line Target Goroutine Wake UP  
200000 Line Target Goroutine Wake UP  
150000 Line Target Goroutine Wake UP  
250000 Line Target Goroutine Wake UP  
300000 Line Target Goroutine Wake UP  
100000 Line Target Goroutine Shut Down  
Answer1 : 100003  
150000 Line Target Goroutine Shut Down  
Answer2 : 150001  
200000 Line Target Goroutine Shut Down  
Answer3 : 200003  
250000 Line Target Goroutine Shut Down  
Answer4 : 250007  
300000 Line Target Goroutine Shut Down  
Answer5 : 300007  
