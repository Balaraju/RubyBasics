
# **Real-Time Example: Using Procs, Lambdas, and Blocks in a Web Application (Ruby on Rails)**  

## **Scenario: Implementing a Flexible Authentication System**  
We are building a **Ruby on Rails authentication system** where different users (Admins, Customers, and Employees) need different authentication strategies.  

---

## **Solution Using Procs, Lambdas, and Blocks**  

### **1️⃣ Using a Proc for Flexible Authentication Rules**  
A `Proc` allows us to store authentication rules for different user roles and reuse them.  

```ruby
AUTH_RULES = {
  admin: Proc.new { |user| user.role == "admin" && user.active? },
  customer: Proc.new { |user| user.role == "customer" && user.verified? },
  employee: Proc.new { |user| user.role == "employee" && user.department.present? }
}

def authenticate(user, role)
  AUTH_RULES[role].call(user)
end

user1 = OpenStruct.new(role: "admin", active?: true)
puts authenticate(user1, :admin)  # Output: true

user2 = OpenStruct.new(role: "customer", verified?: false)
puts authenticate(user2, :customer)  # Output: false
```

✅ **Why use Proc?**  
- We store authentication logic dynamically for different roles.  
- Avoids multiple `if-else` conditions inside methods.  

---

### **2️⃣ Using a Lambda for Strict Parameter Checking**  
A `Lambda` ensures we pass exactly two parameters: a `user` and a `password`.  

```ruby
validate_credentials = ->(user, password) do
  raise ArgumentError, "Invalid input" unless user.is_a?(String) && password.is_a?(String)
  user == "admin" && password == "secure123"
end

puts validate_credentials.call("admin", "secure123")  # Output: true
puts validate_credentials.call("admin")  # Error: wrong number of arguments (given 1, expected 2)
```

✅ **Why use a Lambda?**  
- Enforces strict parameter validation (prevents incorrect calls).  
- Returns only within the lambda scope, preventing method exits.  

---

### **3️⃣ Using Blocks for Dynamic API Callbacks**  
We allow developers to pass custom logging behavior using a **block**.  

```ruby
def login(user)
  puts "Logging in user: #{user[:name]}"
  yield if block_given?  # Execute custom logging if provided
  puts "Login successful"
end

login({ name: "Alice" }) { puts "Custom log: User authenticated" }
```

✅ **Why use a Block?**  
- Allows optional custom logging or behavior.  
- Keeps the method flexible without modifying core logic.  

---

## **Real-World Benefits of Using Procs, Lambdas, and Blocks**

| Feature  | Benefit in a Web Application |
|----------|-----------------------------|
| **Procs** | Store reusable logic (authentication rules, callbacks). |
| **Lambdas** | Enforce strict argument validation (validating credentials, input sanitization). |
| **Blocks** | Provide optional behaviors dynamically (logging, API requests). |

---

## **Final Thoughts**  
- **Procs** are great for flexible authentication rules.  
- **Lambdas** ensure strict validation for login credentials.  
- **Blocks** allow custom logging during authentication.  
