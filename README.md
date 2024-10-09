package main

import ("database/sql","fmt")

 _ "github.com/go-sql-driver/mysql" // Import the MySQL driver
)

func main() {
 // Database connection details
 dbUser := "your_username"
 dbPass := "your_password"
 dbName := "your_database"
 dbHost := "localhost" // Or your database host
 dbPort := "3306"     // Or your database port

 // Build the connection string
 dbSource := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s", dbUser, dbPass, dbHost, dbPort, dbName)

 // Connect to the database
 db, err := sql.Open("mysql", dbSource)
 if err != nil {
  panic(err)
 }
 defer db.Close()

 // Check if the connection is successful
 err = db.Ping()
 if err != nil {
  panic(err)
 }

 fmt.Println("Connected to the database successfully!")

 // Example query
 rows, err := db.Query("SELECT * FROM your_table")
 if err != nil {
  panic(err)
 }
 defer rows.Close()

 // Iterate through the results
 for rows.Next() {
  // Create a new struct or map to hold the row data
  var id int
  var name string

  err := rows.Scan(&id, &name)
  if err != nil {
   panic(err)
  }

  // Do something with the data
  fmt.Printf("ID: %d, Name: %s\n", id, name)
 }

 if err := rows.Err(); err != nil {
  panic(err)
 }
}
