# Domain Testing Report

## Feature: FR-01: Đăng ký tài khoản

# Step-by-Step Explanation

## Step 1 – Input & Output Identification

### Business Objective
To register a new user account with unique credentials and robust password requirements, enabling secure access to the EShop system and transferring the user to the Login page upon success.

### Inputs

| Input | Type | Source | Constraints (Specification) | Constraints (Implementation) |
| :--- | :--- | :--- | :--- | :--- |
| **Họ Tên** (`name`) | String | UI Form / API Body | Mandatory (Required). | Mandatory in form (HTML5 `required`). No backend validation length check. |
| **Email** (`email`) | String | UI Form / API Body | - Mandatory.<br>- Must match valid email format (`user@domain.com`).<br>- Must be unique. | - Mandatory (HTML5 `required` in form).<br>- **Defect (Web GUI)**: Input type is `text` not `email`. No frontend format check.<br>- **Defect (Backend)**: No format check, no uniqueness check. Duplicate emails allowed! |
| **Mật khẩu** (`password`) | String | UI Form / API Body | - Mandatory.<br>- Min 8 characters.<br>- At least 1 uppercase letter.<br>- At least 1 lowercase letter.<br>- At least 1 digit.<br>- At least 1 special character from: `[@, $, !, %, *, ?, &]`. | - Mandatory (HTML5 `required` in form).<br>- **Defect (Web Regex)**: Incorrectly requires whitespace (`\s`) and forbids other special characters (`[A-Za-z\d\s]`).<br>- **Defect (Mobile Regex)**: Accepts any non-alphanumeric (`[^A-Za-z\d]`), which is broader than the specific set.<br>- **Defect (Backend)**: No password strength validation. |
| **Xác nhận mật khẩu** (`confirmPassword`) | String | UI Form only | - Mandatory.<br>- Must match Mật khẩu (`password`). | - **Defect**: Completely missing from both Frontend Web and Frontend Mobile! |

### Outputs

| Output | Description |
| :--- | :--- |
| **Registration Success** | - API Response: `200 OK` with JSON `{"message": "User registered successfully", "id": <user_id>}`.<br>- UI Action: User is redirected to the Login page. |
| **Registration Failure** | - API Response: `400 Bad Request` or `500 Internal Server Error` with JSON `{"error": "<error_message>"}`.<br>- UI Action: Error message displayed on the form (above the Submit button). |

---

## Step 2 – Equivalence Classes

### Condition 1: Họ Tên must be provided
* **Interpretation**: The Họ Tên input field cannot be blank, empty, or contain only whitespaces.
* **Valid Equivalence Classes**:
  * **EC_NAME_VAL_01**: Name contains at least one non-whitespace character.
* **Invalid Equivalence Classes**:
  * **EC_NAME_INV_01**: Name is empty (`""`).
  - **EC_NAME_INV_02**: Name is null or omitted from payload.

### Condition 2: Email must be provided
* **Interpretation**: The email input field cannot be blank or omitted.
* **Valid Equivalence Classes**:
  * **EC_EMAIL_REQ_VAL_01**: Email field is populated.
* **Invalid Equivalence Classes**:
  * **EC_EMAIL_REQ_INV_01**: Email is empty (`""`).
  - **EC_EMAIL_REQ_INV_02**: Email is null or omitted from payload.

### Condition 3: Email must have a valid format (`user@domain.com`)
* **Interpretation**: Email must contain local part, `@` separator, and domain part.
* **Valid Equivalence Classes**:
  * **EC_EMAIL_FMT_VAL_01**: Valid format with single `@` and proper domain (e.g. `test@eshop.com`).
* **Invalid Equivalence Classes**:
  * **EC_EMAIL_FMT_INV_01**: Missing `@` symbol (e.g. `testeshop.com`).
  - **EC_EMAIL_FMT_INV_02**: Missing local part before `@` (e.g. `@eshop.com`).
  - **EC_EMAIL_FMT_INV_03**: Missing domain part after `@` (e.g. `test@`).
  - **EC_EMAIL_FMT_INV_04**: Contains invalid spaces or forbidden characters (e.g. `test @eshop.com`).

### Condition 4: Email must be unique in the system
* **Interpretation**: The email must not already exist in the database of registered users.
* **Valid Equivalence Classes**:
  * **EC_EMAIL_UNIQ_VAL_01**: Email does not match any existing user.
* **Invalid Equivalence Classes**:
  * **EC_EMAIL_UNIQ_INV_01**: Email already registered in the system.

### Condition 5: Mật khẩu must be provided
* **Interpretation**: The password input field cannot be blank or omitted.
* **Valid Equivalence Classes**:
  * **EC_PW_REQ_VAL_01**: Password field is populated.
* **Invalid Equivalence Classes**:
  * **EC_PW_REQ_INV_01**: Password is empty (`""`).
  - **EC_PW_REQ_INV_02**: Password is null or omitted.

### Condition 6: Mật khẩu length must be at least 8 characters
* **Interpretation**: Total character count of the password must be 8 or more.
* **Valid Equivalence Classes**:
  * **EC_PW_LEN_VAL_01**: Length >= 8.
* **Invalid Equivalence Classes**:
  * **EC_PW_LEN_INV_01**: Length < 8.

### Condition 7: Mật khẩu must contain at least 1 uppercase letter
* **Interpretation**: Password has at least one uppercase letter (A-Z).
* **Valid Equivalence Classes**:
  * **EC_PW_UC_VAL_01**: Password contains at least one uppercase letter.
* **Invalid Equivalence Classes**:
  * **EC_PW_UC_INV_01**: Password contains no uppercase letters.

### Condition 8: Mật khẩu must contain at least 1 lowercase letter
* **Interpretation**: Password has at least one lowercase letter (a-z).
* **Valid Equivalence Classes**:
  * **EC_PW_LC_VAL_01**: Password contains at least one lowercase letter.
* **Invalid Equivalence Classes**:
  * **EC_PW_LC_INV_01**: Password contains no lowercase letters.

### Condition 9: Mật khẩu must contain at least 1 digit
* **Interpretation**: Password has at least one digit (0-9).
* **Valid Equivalence Classes**:
  * **EC_PW_DG_VAL_01**: Password contains at least one digit.
* **Invalid Equivalence Classes**:
  * **EC_PW_DG_INV_01**: Password contains no digits.

### Condition 10: Mật khẩu must contain at least 1 special character from standard set: `@`, `$`, `!`, `%`, `*`, `?`, `&`
* **Interpretation**: Password has at least one character belonging to the specific set: `@`, `$`, `!`, `%`, `*`, `?`, `&`.
* **Valid Equivalence Classes**:
  * **EC_PW_SP_VAL_01**: Password contains at least one character from `{@, $, !, %, *, ?, &}`.
* **Invalid Equivalence Classes**:
  * **EC_PW_SP_INV_01**: Password contains no special characters at all.
  - **EC_PW_SP_INV_02**: Password contains special characters, but none from the allowed set (e.g. contains only `#` or `^`)

Note: The specificiation stated in README.md is incredibly vague, where it says "ký tự đặc biệt (`@`, `$`, `!`, `%`, `*`, `?`, `&`)". This could mean either the stated characters are just examples or they're the only permitted special characters. In this case, I lean to the latter interpretation

### Condition 11: Xác nhận mật khẩu must match Mật khẩu (Multi-variable)
* **Interpretation**: The confirmation password field must be identical to the password field.
* **Valid Equivalence Classes**:
  * **EC_CONF_VAL_01**: `confirmPassword` is identical to `password`.
* **Invalid Equivalence Classes**:
  * **EC_CONF_INV_01**: `confirmPassword` differs from `password`.

---

## Step 3 – Representative Test Cases

### Selection Strategy
- **Valid Classes**: Combined into a single positive test case (`TC_DOM_VAL_01`) to verify the normal flow when all constraints are successfully satisfied.
- **Invalid Classes**: Isolated so that each invalid test case violates exactly one invalid equivalence class constraint, preventing masking of defects.

| Test Case ID | Technique | Partitions Covered | Inputs | Expected Outcome | Actual Result | Verdict
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC_DOM_VAL_01** | Domain | All Valid classes:<br>`EC_NAME_VAL_01`, `EC_EMAIL_REQ_VAL_01`, `EC_EMAIL_FMT_VAL_01`, `EC_EMAIL_UNIQ_VAL_01`, `EC_PW_REQ_VAL_01`, `EC_PW_LEN_VAL_01`, `EC_PW_UC_VAL_01`, `EC_PW_LC_VAL_01`, `EC_PW_DG_VAL_01`, `EC_PW_SP_VAL_01`, `EC_CONF_VAL_01` | `name`: "Nguyen Van A"<br>`email`: "new_user@eshop.com"<br>`password`: "Password123!"<br>`confirmPassword`: "Password123!" | **Success**: Registration completes.<br>- Status 200/Success.<br>- Redirection to Login page. | **Failure**: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | FAIL
| **TC_DOM_INV_01** | Domain | `EC_NAME_INV_01` | `name`: ""<br>`email`: "user@eshop.com"<br>`password`: "Password123!" | **Failure**: A "Please fill out this field" popout | A "Please fill out this field" popout | PASS
| **TC_DOM_INV_02** | Domain | `EC_EMAIL_REQ_INV_01` | `name`: "Nguyen Van A"<br>`email`: ""<br>`password`: "Password123!" | **Failure**: A "Please fill out this field" popout | A "Please fill out this field" popout | PASS
| **TC_DOM_INV_03** | Domain | `EC_EMAIL_FMT_INV_01` | `name`: "Nguyen Van A"<br>`email`: "user_eshop.com"<br>`password`: "Password 123"| **Failure** "Email không đúng định dạng." | User gets redirected to the sign in page as the account has been successfully registered | FAIL
| **TC_DOM_INV_04** | Domain | `EC_EMAIL_FMT_INV_02` | `name`: "Nguyen Van A"<br>`email`: "@eshop.com"<br>`password`: "Password 123"| **Failure** "Email không đúng định dạng." | User gets redirected to the sign in page as the account has been successfully registered | FAIL
| **TC_DOM_INV_05** | Domain | `EC_EMAIL_FMT_INV_03` | `name`: "Nguyen Van A"<br>`email`: "user@"<br>`password`: "Password 123" | **Failure** "Email không đúng định dạng." | User gets redirected to the sign in page as the account has been successfully registered | FAIL
| **TC_DOM_INV_06** | Domain | `EC_EMAIL_FMT_INV_04` | `name`: "Nguyen Van A"<br>`email`: "user @eshop.com"<br>`password`: "Password 123" | **Failure** "Email không đúng định dạng." | User gets redirected to the sign in page as the account has been successfully registered | FAIL
| **TC_DOM_INV_07** | Domain | `EC_EMAIL_UNIQ_INV_01` | `name`: "Nguyen Van A"<br>`email`: "test@eshop.com" (exists)<br>`password`: "Password 123" | **Failure** "Email đã được sử dụng." | User gets redirected to the sign in page as the account has been successfully registered | FAIL
| **TC_DOM_INV_08** | Domain | `EC_PW_REQ_INV_01` | `name`: "Nguyen Van A"<br>`email`: "user@eshop.com"<br>`password`: "" | **Failure**: "Please fill out this field" popout | A "Please fill out this field" popout | PASS
| **TC_DOM_INV_09** | Domain | `EC_PW_LEN_INV_01` | `name`: "Nguyen Van A"<br>`email`: "user@eshop.com"<br>`password`: "Pas12!" | **Failure**: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | **Failure**: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | PASS
| **TC_DOM_INV_10** | Domain | `EC_PW_UC_INV_01` | `name`: "Nguyen Van A"<br>`email`: "user@eshop.com"<br>`password`: "password123!" | **Failure** Error message: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | **Failure**: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | PASS
| **TC_DOM_INV_11** | Domain | `EC_PW_LC_INV_01` | `name`: "Nguyen Van A"<br>`email`: "user@eshop.com"<br>`password`: "PASSWORD123!" | **Failure** Error message: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | **Failure**: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | PASS
| **TC_DOM_INV_12** | Domain | `EC_PW_DG_INV_01` | `name`: "Nguyen Van A"<br>`email`: "user@eshop.com"<br>`password`: "Password!" | **Failure** Error message: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | **Failure**: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | PASS
| **TC_DOM_INV_13** | Domain | `EC_PW_SP_INV_01` | `name`: "Nguyen Van A"<br>`email`: "user@eshop.com"<br>`password`: "Password123"<br>`confirmPassword`: "Password123" | **Failure** Error message: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | **Failure**: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | PASS
| **TC_DOM_INV_14** | Domain | `EC_PW_SP_INV_02` | `name`: "Nguyen Van A"<br>`email`: "user@eshop.com"<br>`password`: "Password123#" | **Failure**: "Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT." | **Failure**: "Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT." | PASS

Note: TC_DOM_INV from 3 to 7 will use "Password 123" as the password to show the actual results, despite violating the stated specifications. Using an actual valid password like "Password123!" will otherwise always trigger the **Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT.** notification

## AI Gap Analysis (Domain Testing - FR-01)

After thorough examination, the AI (LLM) showed several limitations

- **Gap #1: Password confirmation test case added despite being unimplemented** AI added the matching confirm password test case, while forgetting that the implementation is completely absent from the frontend
- **Why:** The AI is too focused on the specification stated on the README.md, despite having stated earlier that there is a discrepancy between implementation and the specification
- **Gap #2: Using "Password123!" for invalid email test cases** AI used the aforementioned password for such test cases, despite knowing that the implemented regex was incorrect (the inaccuracy makes testing invalid emails more difficult)
- **Why:** The AI is again, too focused on the password specification stated on the README.md, despite having stated earlier that there is a discrepancy between implementation and the specification

---

## Feature: FR-07: Giỏ hàng (Shopping Cart)

# Step-by-Step Explanation

### Business Objective
To display the list of products in the user's shopping cart with correct item details, handle quantity adjustments via `+`/`-` buttons, group duplicate products, prompt confirmation before deletion, compute correct subtotals under the label "Tổng cộng", and show a clear empty state when no items exist.

### Inputs

| Input | Type | Source | Constraints (Specification) | Constraints (Implementation) |
| :--- | :--- | :--- | :--- | :--- |
| **Số lượng** (`quantity`) | Integer | UI Input / Buttons | - Must be an integer >= 1.<br>- Modified via `+` and `-` buttons in the cart table. | - **Defect (Web)**: Entirely static text; no inputs or `+`/`-` adjustment buttons exist.<br>- **Defect (Mobile)**: Handled via text input (not buttons). The text input handler contains a major math defect: setting the quantity to `parsed + 1` instead of `parsed`. |
| **Thêm vào giỏ** (`addToCart` event) | Event (Product Object + Qty) | Product Detail Page (FR-06) | - If the product already exists in the cart, add the new quantity to the existing quantity.<br>- Do not create a new row. | - **Defect (Web)**: Context `addToCart` always appends a new object, creating separate rows for the same product.<br>- Mobile: Correctly groups duplicate items. |
| **Xóa sản phẩm** (`removeFromCart` click) | Click Event | Cart Table Action | Clicking must show a confirmation dialog before deleting. | - **Defect (Web & Mobile)**: Instantly deletes the item without displaying any confirmation dialog. |

### Outputs

| Output | Description |
| :--- | :--- |
| **Cart List Layout** | Displays a table containing: **Sản phẩm**, **Đơn giá**, **Số lượng** (with +/- buttons), **Thành tiền** (subtotal: `price * quantity`), and **Thao tác** (Delete button). |
| **Confirmation Dialog** | A modal dialog stating: "Bạn có chắc chắn muốn xóa sản phẩm này?" with Confirm/Cancel actions. |
| **Total Price Display** | Displayed at the bottom of the cart showing the summation of all line items under the exact label **"Tổng cộng"**. |
| **Empty State Display** | Displayed when the cart has 0 items. Must include a clear friendly message and an illustration/icon. |

---

## Step 2 – Equivalence Classes

### Condition 1: Quantity value of cart items
* **Interpretation**: The adjusted quantity of a product in the cart must be a positive integer >= 1.
* **Valid Equivalence Classes**:
  * **EC_QTY_VAL_01**: Quantity is an integer > 1.
  - **EC_QTY_VAL_02**: Quantity is exactly 1 (minimum threshold).
* **Invalid Equivalence Classes**:
  * **EC_QTY_INV_01**: Quantity is 0.
  - **EC_QTY_INV_02**: Quantity is a negative integer (e.g. -5).
  - **EC_QTY_INV_03**: Quantity is a floating-point decimal (e.g. 1.5).
  - **EC_QTY_INV_04**: Quantity is non-numeric/empty.

### Condition 2: Add same product to cart (Grouping)
* **Interpretation**: When adding a product that is already present in the cart, the quantities must merge.
* **Valid Equivalence Classes**:
  * **EC_ADD_NEW_VAL_01**: Adding a product not already in the cart creates a new row.
  - **EC_ADD_EXIST_VAL_01**: Adding a product already in the cart merges quantities and updates the existing row.

### Condition 3: Remove item from cart (Confirmation)
* **Interpretation**: A confirmation dialog must interrupt the deletion action.
* **Valid Equivalence Classes**:
  * **EC_DEL_CONF_VAL_01**: Click delete -> Confirm dialog -> Click "Yes" -> Item is removed.
  - **EC_DEL_CANCEL_VAL_01**: Click delete -> Confirm dialog -> Click "No" -> Item remains in cart.

### Condition 4: Empty cart state representation
* **Interpretation**: When the cart is empty, a specific friendly template with an illustration must be shown.
* **Valid Equivalence Classes**:
  * **EC_EMPTY_VAL_01**: Cart is empty -> Displays text message, illustration/icon, and home link.
* **Invalid Equivalence Classes**:
  * **EC_EMPTY_INV_01**: Cart is not empty -> Displays the standard list table.

### Condition 5: Total amount display details
* **Interpretation**: The total price must show a mathematically correct sum under the exact label "Tổng cộng".
* **Valid Equivalence Classes**:
  * **EC_TOTAL_VAL_01**: Total matches `sum(price * quantity)` for all items.
  - **EC_TOTAL_LABEL_VAL_01**: The label text reads exactly "Tổng cộng".
* **Invalid Equivalence Classes**:
  * **EC_TOTAL_LABEL_INV_01**: The label text displays something else (e.g. "Tổng tạm tính").

---

## Step 3 – Representative Test Cases

### Selection Strategy
- **Valid Classes**: Combined into a single end-to-end positive flow (`TC_DOM_VAL_01` & `TC_DOM_VAL_02`) to optimize test execution.
- **Invalid Classes**: Isolated one by one in invalid test cases to ensure that failing validation rules are correctly identified.

| Test Case ID | Technique | Partitions Covered | Inputs | Expected Outcome | Actual Result | Verdict
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC_DOM_VAL_01** | Domain | `EC_EMPTY_VAL_01`, `EC_ADD_NEW_VAL_01`, `EC_ADD_EXIST_VAL_01`, `EC_QTY_VAL_01`, `EC_TOTAL_VAL_01`, `EC_TOTAL_LABEL_VAL_01`, `EC_EMPTY_INV_01`, `EC_TOTAL_LABEL_INV_01`, `EC_QTY_VAL_02` | 1. View empty cart.<br>2. Add Product A (Qty: 1) first time.<br>3. Add Product A (Qty: 2) second time.<br>4. Add Product B (Qty: 3). | 1. Empty state displays message + illustration.<br>2. Product A row added (Qty 1).<br>3. Product A Qty updates to 3 (no new row).<br>4. Product B row added (Qty 3).<br>5. Total label is "Tổng cộng". Total price = `(PriceA * 3) + (PriceB * 3)`. | 1. Empty state displays message + illustration.<br>2. Product A row added (Qty 1).<br>3. Three duplicates of Product A is added.<br>4. Product B row added (Qty 3).<br>5. Total label is "Tổng tạm tính". Total price = `(PriceA * 3) + (PriceB * 3)`. | FAIL
| **TC_DOM_VAL_02** | Domain | `EC_DEL_CANCEL_VAL_01` | 1. Cart contains Product A.<br>2. Click Delete Product A.<br>3. Click "No" / "Cancel" on dialog. | 1. Confirmation dialog appears.<br>2. Dialog closes. Product A is NOT deleted from cart. | The product is removed on click without confirmation | FAIL
| **TC_DOM_VAL_03** | Domain | `EC_DEL_CONF_VAL_01` | 1. Cart contains Product A.<br>2. Click Delete Product A.<br>3. Click "Yes" / "Confirm" on dialog. | 1. Confirmation dialog appears.<br>2. Product A is removed. Cart total recalculates. | The product is removed on click without confirmation | FAIL
| **TC_DOM_INV_01** | Domain | `EC_QTY_INV_01` | 1. Add product A using `Xem chi tiết`.<br>2. Set Product A quantity to `0`. | **Failure**: Value is rejected | The quantity of Product A is 0 | FAIL
| **TC_DOM_INV_02** | Domain | `EC_QTY_INV_02` | 1. Add product A using `Xem chi tiết`.<br>2. Set Product A quantity to `-5`. | **Failure**: Value is rejected. | The quantity of Product A is -5 | FAIL
| **TC_DOM_INV_03** | Domain | `EC_QTY_INV_03` | 1. Add product A using `Xem chi tiết`<br>2. Set Product A quantity to `2.5`. | **Failure**: Value is rejected. | The resulting quantity is truncated to 2 | FAIL
| **TC_DOM_INV_04** | Domain | `EC_QTY_INV_04` | 1. Add product A using `Xem chi tiết`<br>2. Delete the quantity field (empty string). | **Failure**: Shows validation error. | Quantity is NaN | FAIL

## AI Gap Analysis (Domain Testing - FR-07)

After thorough examination, the AI (LLM) showed several limitations

- **Gap #1: AI forgetting to write addition covered partitions for TC_DOM_VAL_01** AI missed the `EC_EMPTY_INV_01`, `EC_TOTAL_LABEL_INV_01`, `EC_QTY_VAL_02` partitons for TC_DOM_VAL_01
- **Why:** It can be explained that AI has a context window limit where the possible details are omitted due to it
- **Gap #2: AI using increment (ie using the increment button)** AI responded with an answer that sounds like using the non-existent `+`/`-` button
- **Why:** The AI read is too focused on the specification document while forgetting the actual implementation is lacking such feature
- **Gap #3: AI using 'or' for TC_DOM_INV_[01 to 04] (Non-deterministic Outcome)** AI presented an expected outcome with one of the results, violating ISTQB'principle of determinism
- **Why:** LLMs are inherently probabilisitc. When dealing with cases that could have multiple interpretation about the expected outcome, AI has the tendency of stating every possible solution rather than committing to one result. This shows AI struggle in deterministic decision-making when lacking concrete information

---

## Feature: FR-13 (Dashboard)

# Step-by-Step Explanation

## Step 1 – Input & Output Identification

### Business Objective
To display administrative metrics on the Dashboard, specifically calculating and rendering:
1. **Tổng doanh thu (Total Revenue)**: Sum of `total_amount` only for orders with `status = 'delivered'`.
2. **Tổng số đơn hàng (Total Orders)**: Total count of all orders in the system.

### Inputs

| Input | Type | Source | Constraints (Specification) | Constraints (Implementation) |
| :--- | :--- | :--- | :--- | :--- |
| **Danh sách Đơn hàng** (`orders` array) | Array of Objects | API response (`GET /api/admin/orders`) | - Each order must contain numeric `total_amount` and status string.<br>- Status values: `pending`, `confirmed`, `shipping`, `delivered`, `canceled`. | - Loads the raw database orders table from SQLite via `server.js`.<br>- Lacks schema/input validation on the backend for negative amounts or unknown status codes. |
| **Trạng thái đơn hàng** (`status`) | String | Order Attribute | Must be exactly `'delivered'` to be counted in revenue. | Evaluated in the array reduction loop in `App.jsx`. |
| **Giá trị đơn hàng** (`total_amount`) | Integer | Order Attribute | Must be an integer >= 0. | - **Defect**: Multiplied by 2 during frontend summation, displaying double the true revenue. |

### Outputs

| Output | Description |
| :--- | :--- |
| **Tổng doanh thu (Delivered)** | Numeric sum display of all delivered orders, styled green, formatted with thousand separators and `₫` currency symbol. |
| **Tổng số đơn hàng** | Integer counter showing the length of the orders array. |

---

## Step 2 – Equivalence Classes

### Condition 1: Order Status for Revenue Calculation
* **Interpretation**: Only orders with the exact status value of `'delivered'` should contribute to the total revenue. Other statuses should be skipped, but still counted as orders.
* **Valid Equivalence Classes**:
  * **EC_STATUS_VAL_DELIVERED**: Status is `'delivered'` (Contributes to revenue).
  - **EC_STATUS_VAL_PENDING**: Status is `'pending'` (0 revenue contribution).
  - **EC_STATUS_VAL_CONFIRMED**: Status is `'confirmed'` (0 revenue contribution).
  - **EC_STATUS_VAL_SHIPPING**: Status is `'shipping'` (0 revenue contribution).
  - **EC_STATUS_VAL_CANCELED**: Status is `'canceled'` (0 revenue contribution).
* **Invalid Equivalence Classes**:
  * **EC_STATUS_INV_UNKNOWN**: Status is an unsupported or corrupt value (e.g. `'returned'` or `'unknown'`).

### Condition 2: Order Total Amount (`total_amount`)
* **Interpretation**: The total value of an order must be a non-negative number.
* **Valid Equivalence Classes**:
  * **EC_AMT_VAL_POS**: Amount is > 0 (e.g. 500,000).
  - **EC_AMT_VAL_ZERO**: Amount is exactly 0.
* **Invalid Equivalence Classes**:
  * **EC_AMT_INV_NEG**: Amount is negative (e.g. -100,000).
  - **EC_AMT_INV_FLOAT**: Amount is decimal/float (e.g. 1000.5).
  - **EC_AMT_INV_NON_NUMERIC**: Amount is non-numeric (null, string, or undefined).

### Condition 3: Orders collection state (Database state)
* **Interpretation**: The metrics must react correctly whether the store has no orders or contains multiple entries.
* **Valid Equivalence Classes**:
  * **EC_DB_EMPTY**: Database contains 0 orders.
  - **EC_DB_POPULATED**: Database contains >= 1 orders.

---

## Step 3 – Representative Test Cases

### Selection Strategy
- **Valid Classes**: Combined into test case `TC_DOM_VAL_02` where a dataset of orders with all possible statuses is evaluated to verify correct filtration and summation.
- **Invalid Classes**: Tested in isolation (`TC_DOM_INV_01` & `TC_DOM_INV_02`) to ensure incorrect data format handling behaves predictably.

| Test Case ID | Technique | Partitions Covered | Inputs | Expected Outcome | Actual Result | Verdict
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC_DOM_VAL_01** | Domain | `EC_DB_EMPTY` | `orders` = `[]` | - **Tổng doanh thu**: 0 ₫<br>- **Tổng số đơn hàng**: 0 | - **Tổng doanh thu**: 0 ₫<br>- **Tổng số đơn hàng**: 0 | PASS
| **TC_DOM_VAL_02** | Domain | `EC_DB_POPULATED`, `EC_STATUS_VAL_DELIVERED`, `EC_STATUS_VAL_PENDING`, `EC_STATUS_VAL_CONFIRMED`, `EC_STATUS_VAL_SHIPPING`, `EC_STATUS_VAL_CANCELED`, `EC_AMT_VAL_POS`, `EC_AMT_VAL_ZERO` | `orders` = [<br>  { id: 1, total_amount: 30000000, status: "delivered" },<br>  { id: 2, total_amount: 28000000, status: "pending" },<br>  { id: 3, total_amount: 45000000, status: "confirmed" },<br>  { id: 4, total_amount: 600000, status: "shipping" },<br>  { id: 5, total_amount: 400000, status: "canceled" },<br>  { id: 6, total_amount: 0, status: "delivered" }<br>] | - **Tổng doanh thu**: 30.000.000 ₫ (Sum of 30 million + 0)<br>- **Tổng số đơn hàng**: 6 | - **Tổng doanh thu**: 60.000.000 ₫ <br>- **Tổng số đơn hàng**: 6 | FAIL
| **TC_DOM_INV_01** | Domain | `EC_AMT_INV_NEG` | `orders` = [<br>  { id: 1, total_amount: -30000000, status: "delivered" }<br>] | - **System Action**: Displays error, rejects database update (negative values shouldn't bypass DB schema checks). | - **Tổng doanh thu**: -60.000.000 ₫ <br>- **Tổng số đơn hàng**: 1 | FAIL
| **TC_DOM_INV_02** | Domain | `EC_STATUS_INV_UNKNOWN` | `orders` = [<br>  { id: 1, total_amount: 30000000, status: "unknown_status" }<br>] | - **Tổng doanh thu**: 0 ₫ (Order ignored)<br>- **Tổng số đơn hàng**: 1 | - **Tổng doanh thu**: 0 ₫ (Order ignored)<br>- **Tổng số đơn hàng**: 1 | PASS
| **TC_DOM_INV_03** | Domain | `EC_AMT_INV_FLOAT` | `orders` = [<br>  { id: 1, total_amount: 30000000.6, status: "delivered" }<br>] | - **System Action**: Displays error, rejects database update (float values shouldn't bypass DB schema checks). | - **Tổng doanh thu**: 60000001.2 ₫ <br>- **Tổng số đơn hàng**: 1 | FAIL
| **TC_DOM_INV_04** | Domain | `EC_AMT_INV_NON_NUMERIC` | `orders` = [<br>  { id: 1, total_amount: NaN, status: "delivered" }<br>] | - **System Action**: Displays error, rejects database update (negative values shouldn't bypass DB schema checks). | - **Tổng doanh thu**: 0 ₫ <br>- **Tổng số đơn hàng**: 1 | FAIL

## AI Gap Analysis (Domain Testing - FR-14)

After thorough examination, the AI (LLM) showed several limitations

- **Gap #1: AI missing test cases for the last two partitions** AI forgot to add test cases covering the `EC_AMT_INV_FLOAT`, `EC_AMT_INV_NON_NUMERIC` partitions, where human has to add it in the end
- **Why:** The AI's context memory probably reached its limits, making it forgetting several contexts that were loaded earlier
- **Gap #2: AI using 'or' for TC_DOM_INV_01 (Non-deterministic Outcome)** AI presented an expected outcome with one of the results, violating ISTQB'principle of determinism
- **Why:** LLMs are inherently probabilisitc. When dealing with cases that could have multiple interpretation about the expected outcome, AI has the tendency of stating every possible solution rather than committing to one result. This shows AI struggle in deterministic decision-making when lacking concrete information

---

## Feature: FR-04 (Quản lý hồ sơ cá nhân - Mobile Application)

# Step-by-Step Explanation

## Step 1 – Input & Output Identification

### Business Objective
To allow logged-in users to update their personal information (Họ Tên, Số điện thoại, Địa chỉ giao hàng mặc định) on the mobile application while strictly preventing modifications to their registration email or system role.

### Inputs

| Input | Type | Source | Constraints (Specification) | Constraints (Implementation) |
| :--- | :--- | :--- | :--- | :--- |
| **Họ Tên** (`name`) | String | UI Form | Mandatory. | Directly updated without length verification. |
| **Số điện thoại** (`phone`) | String | UI Form | - Mandatory.<br>- Must start with `0`.<br>- Must have a length of 10 or 11 digits. | - **Defect (Validation Bug)**: Regulated by regex `^[1-9][0-9]{8,9}$`. It rejects any number starting with `0` and restricts the total length to 9 or 10 digits instead of 10 or 11. |
| **Địa chỉ giao hàng** (`shippingAddress`) | String | UI Form | Optional/Mandatory. | Directly updated without validation. |
| **Vai trò** (`role`) | String | API Body | Must not be modifiable by the user (Role changes prohibited). | - **Defect (Security Vulnerability)**: The backend API checks for and updates the `role` parameter if sent in the HTTP request body. |

### Outputs

| Output | Description |
| :--- | :--- |
| **Profile Update Success** | - UI Alert: "Cập nhật thành công!"<br>- DB: User's profile is updated in the users database. |
| **Profile Update Failure** | - UI Alert: "Số điện thoại không hợp lệ. Vui lòng nhập đúng 9-10 chữ số." (Implementation message) or "Lỗi cập nhật". |

---

## Step 2 – Equivalence Classes

### Condition 1: Phone number starting digit
* **Interpretation**: The phone number must start with the character `0` according to the business specification.
* **Valid Equivalence Classes**:
  * **EC_PHONE_START_VAL_01**: Phone number starts with `0` (e.g. `0912345678`).
* **Invalid Equivalence Classes**:
  * **EC_PHONE_START_INV_01**: Phone number starts with a non-zero digit `1-9` (e.g. `912345678`).
  - **EC_PHONE_START_INV_02**: Phone number starts with a non-digit character (e.g. `+84...`).

### Condition 2: Phone number length (Ordered Partition)
* **Interpretation**: The phone number must contain exactly 10 or 11 digits.
* **Valid Equivalence Classes**:
  * **EC_PHONE_LEN_VAL_01**: Length is 10 or 11 digits.
* **Invalid Equivalence Classes**:
  * **EC_PHONE_LEN_INV_SHORT**: Length < 10 digits (e.g. 9 digits).
  - **EC_PHONE_LEN_INV_LONG**: Length > 11 digits (e.g. 12 digits).

### Condition 3: Phone number character composition
* **Interpretation**: The phone number must contain only numeric characters (`0-9`).
* **Valid Equivalence Classes**:
  * **EC_PHONE_CHAR_VAL_01**: Phone number contains only digits.
* **Invalid Equivalence Classes**:
  * **EC_PHONE_CHAR_INV_01**: Phone number contains alphabetic or special characters (e.g. `0912abc345`, `0912-345678`).

### Condition 4: User Role Authorization (Security Check)
* **Interpretation**: Users cannot change their privilege level (`role`) through any request.
* **Valid Equivalence Classes**:
  * **EC_ROLE_VAL_01**: Payload does not attempt to modify `role` (or updates remain identical to the original).
* **Invalid Equivalence Classes**:
  * **EC_ROLE_INV_01**: Payload contains a different `role` field (e.g. `role: "admin"`).

---

## Step 3 – Representative Test Cases

### Selection Strategy
- **Valid Classes**: Combined into `TC_DOM_VAL_01` to test the standard profile edit under correct constraints.
- **Invalid Classes**: Tested in isolation (`TC_DOM_INV_01` to `TC_DOM_INV_06`) to ensure that failing validation rules are correctly identified.

| Test Case ID | Technique | Partitions Covered | Inputs | Expected Outcome (Specification) | Actual Result | Verdict
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC_DOM_VAL_01** | Domain | `EC_PHONE_START_VAL_01`<br>`EC_PHONE_LEN_VAL_01`<br>`EC_PHONE_CHAR_VAL_01`<br>`EC_ROLE_VAL_01` | `name`: "Nguyen Van A"<br>`phone`: "0912345678"<br>`shippingAddress`: "123 Le Loi"<br>`role`: *omitted* | **Success**: Profile is updated. Alerts "Cập nhật thành công!". | **Failure (Defect)**: Alert "Số điện thoại không hợp lệ. Vui lòng nhập đúng 9-10 chữ số." (Fails regex). | FAIL
| **TC_DOM_INV_01** | Domain | `EC_PHONE_START_INV_01` | `name`: "Nguyen Van A"<br>`phone`: "912345678"<br>`shippingAddress`: "123 Le Loi" | **Failure**: Rejected. Alerts phone number invalid. | **Success (Defect)**: Profile is updated successfully. | FAIL
| **TC_DOM_INV_02** | Domain | `EC_PHONE_START_INV_02` | `name`: "Nguyen Van A"<br>`phone`: "+0912345678" | **Failure**: Rejected. Alerts phone number invalid. | **Failure**: Alerts phone number invalid. | PASS
| **TC_DOM_INV_03** | Domain | `EC_PHONE_LEN_INV_SHORT` | `name`: "Nguyen Van A"<br>`phone`: "0912" | **Failure**: Rejected. Alerts phone number invalid. | **Failure**: Alerts phone number invalid. | PASS
| **TC_DOM_INV_04** | Domain | `EC_PHONE_LEN_INV_LONG` | `name`: "Nguyen Van A"<br>`phone`: "091234567890" | **Failure**: Rejected. Alerts phone number invalid. | **Failure**: Alerts phone number invalid. | PASS
| **TC_DOM_INV_05** | Domain | `EC_PHONE_CHAR_INV_01` | `name`: "Nguyen Van A"<br>`phone`: "091234567a" | **Failure**: Rejected. Alerts phone number invalid. | **Failure**: Alerts phone number invalid. | PASS
| **TC_DOM_INV_06** | Domain | `EC_ROLE_INV_01` | *Direct API body update:*<br>`name`: "Nguyen Van A"<br>`phone`: "0912345678"<br>`role`: "admin" | **Failure**: API ignores `role` update or returns `403 Forbidden` / `400 Bad Request`. | **Privilege Escalation (Defect)**: Backend API updates the user's role to `'admin'`, bypassing access controls. | FAIL

---

# Boundary Value Analysis

## Feature: FR-01: Đăng ký tài khoản

BVA focuses on the boundary limit of the ordered equivalence partition: **Mật khẩu Length** (Constraint: `length >= 8`).

### Ordered Partition Boundary Check

| Partition | LB | LB−1 | LB+1 | UB−1 | UB | UB+1 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Password Length** (>= 8) | **8** | **7** | **9** | *N/A* | *N/A* | *N/A* |

- **Lower Boundary (LB = 8)**: Minimum valid length. Verifies the system accepts the exact lowest threshold.
- **LB - 1 (Length = 7)**: Invalid value just outside the valid partition. Checks that the boundary constraint prevents short inputs.
- **LB + 1 (Length = 9)**: Valid value just inside the valid partition. Verifies inputs slightly above the boundary work correctly.

### BVA Test Cases

| Test Case ID | Technique | Boundary Covered | Inputs | Expected Outcome | Actual Result | Verdict
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | 
| **TC_BVA_01** | BVA | Password Length = LB - 1 (7) | `name`: "Nguyen Van A"<br>`email`: "bva7@eshop.com"<br>`password`: "Pswd12!"<br>`confirmPassword`: "Pswd12!" | **Failure**: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | **Failure**: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | PASS
| **TC_BVA_02** | BVA | Password Length = LB (8) | `name`: "Nguyen Van A"<br>`email`: "bva8@eshop.com"<br>`password`: "Pswd123!"<br>`confirmPassword`: "Pswd123!" | **Success**: User gets directed back to the login screen. | **Failure**: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. | FAIL
| **TC_BVA_03** | BVA | Password Length = LB + 1 (9) | `name`: "Nguyen Van A"<br>`email`: "bva9@eshop.com"<br>`password`: "Pswd1234!"<br>`confirmPassword`: "Pswd1234!" | **Success**: User gets directed back to the login screen. | **Failure**: Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT. || FAIL

## Feature: FR-07: Giỏ hàng (Shopping Cart)

BVA focuses on the lower boundary limit of the ordered equivalence partition: **Cart Item Quantity** (Constraint: `quantity >= 1`).

### Ordered Partition Boundary Check

| Partition | LB | LB−1 | LB+1 | UB−1 | UB | UB+1 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Cart Item Quantity** (>= 1) | **1** | **0** | **2** | *N/A* | *N/A* | *N/A* |

- **Lower Boundary (LB = 1)**: The absolute minimum items allowed in a cart row.
- **LB - 1 (Value = 0)**: Invalid. Reducing below 1 should trigger deletion confirmation or be rejected.
- **LB + 1 (Value = 2)**: Valid. Verifies the basic incremental step works correctly.

### BVA Test Cases

| Test Case ID | Technique | Boundary Covered | Inputs | Expected Outcome | Actual Result | Verdict
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC_BVA_01** | BVA | Cart Quantity = LB - 1 (0) | 1. Add product A using `Xem chi tiết`.<br>2. Set Product A quantity to `0`. | **Outcome**: System rejects the value. | Product A's quantity is 0 | FAIL
| **TC_BVA_02** | BVA | Cart Quantity = LB (1) | 1. Add product A using `Xem chi tiết`.<br>2. Set Product A quantity to `1`. | **Outcome**: Accepted. Product A subtotal = `price * 1`. | Product A subtotal is `price * 1` | PASS
| **TC_BVA_03** | BVA | Cart Quantity = LB + 1 (2) | 1. Add product A using `Xem chi tiết`.<br>2. Set Product A quantity to `2`. | **Outcome**: Accepted. Product A subtotal = `price * 2`. | Product A subtotal = `price * 2`. | PASS

## AI Gap Analysis (BVA - FR-07)

After thorough examination, the AI (LLM) showed several limitations

- **Gap #1: AI using 'or' for TC_BVA_01 (Non-deterministic Outcome)** AI presented an expected outcome with one of the results, violating ISTQB'principle of determinism
- **Why:** LLMs are inherently probabilisitc. When dealing with cases that could have multiple interpretation about the expected outcome, AI has the tendency of stating every possible solution rather than committing to one result. This shows AI struggle in deterministic decision-making when lacking concrete information

## Feature: FR-13: Dashboard

BVA focuses on two ordered partitions:
1. **Total amount of delivered orders** (Constraint: `total_amount >= 0`).
2. **Total order count in system** (Constraint: `count >= 0`).

### Ordered Partition 1: Delivered Order Amount

| Partition | LB | LB−1 | LB+1 | UB−1 | UB | UB+1 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Delivered Order Amount** (>= 0) | **0** | **-1** | **1** | *N/A* | *N/A* | *N/A* |

### Ordered Partition 2: Total Order Count

| Partition | LB | LB−1 | LB+1 | UB−1 | UB | UB+1 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Total Order Count** (>= 0) | **0** | *N/A* | **1** | *N/A* | *N/A* | *N/A* |

### BVA Test Cases

| Test Case ID | Technique | Boundary Covered | Inputs | Expected Outcome | Actual Result | Verdict
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC_BVA_01** | BVA | Delivered Amount = LB - 1 (-1) | `orders` = [<br>  { id: 1, total_amount: -1, status: "delivered" }<br>] | **Outcome**: System flags validation error, database blocks insert | **Outcome**: Total Revenue = -2 ₫, Total Orders = 1. | FAIL
| **TC_BVA_02** | BVA | Delivered Amount = LB (0) | `orders` = [<br>  { id: 1, total_amount: 0, status: "delivered" }<br>] | **Outcome**: Total Revenue = 0 ₫, Total Orders = 1. | **Outcome**: Total Revenue = 0 ₫, Total Orders = 1 | PASS
| **TC_BVA_03** | BVA | Delivered Amount = LB + 1 (1) | `orders` = [<br>  { id: 1, total_amount: 1, status: "delivered" }<br>] | **Outcome**: Total Revenue = 1 ₫, Total Orders = 1. | **Outcome**: Total Revenue = 2 ₫, Total Orders = 1. | FAIL
| **TC_BVA_04** | BVA | Order Count = LB (0) | `orders` = `[]` | **Outcome**: Total Revenue = 0 ₫, Total Orders = 0. |
| **TC_BVA_05** | BVA | Order Count = LB + 1 (1) | `orders` = [<br>  { id: 1, total_amount: 30000000, status: "delivered" }<br>] | **Outcome**: Total Revenue = 60.000.000 ₫, Total Orders = 1. | FAIL

## AI Gap Analysis (BVA - FR-13)

After thorough examination, the AI (LLM) showed several limitations

- **Gap #1: AI using 'or' for TC_DOM_INV_[01 to 04] (Non-deterministic Outcome)** AI presented an expected outcome with one of the results, violating ISTQB'principle of determinism
- **Why:** LLMs are inherently probabilisitc. When dealing with cases that could have multiple interpretation about the expected outcome, AI has the tendency of stating every possible solution rather than committing to one result. This shows AI struggle in deterministic decision-making when lacking concrete information

## Feature: FR-07: Giỏ hàng (Shopping Cart)

BVA focuses on the lower boundary limit of the ordered equivalence partition: **Cart Item Quantity** (Constraint: `quantity >= 1`).

### Ordered Partition Boundary Check

| Partition | LB | LB−1 | LB+1 | UB−1 | UB | UB+1 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Cart Item Quantity** (>= 1) | **1** | **0** | **2** | *N/A* | *N/A* | *N/A* |

- **Lower Boundary (LB = 1)**: The absolute minimum items allowed in a cart row.
- **LB - 1 (Value = 0)**: Invalid. Reducing below 1 should trigger deletion confirmation or be rejected.
- **LB + 1 (Value = 2)**: Valid. Verifies the basic incremental step works correctly.

### BVA Test Cases

| Test Case ID | Technique | Boundary Covered | Inputs | Expected Outcome | Actual Result | Verdict
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC_BVA_01** | BVA | Cart Quantity = LB - 1 (0) | 1. Add product A using `Xem chi tiết`.<br>2. Set Product A quantity to `0`. | **Outcome**: System rejects the value. | Product A's quantity is 0 | FAIL
| **TC_BVA_02** | BVA | Cart Quantity = LB (1) | 1. Add product A using `Xem chi tiết`.<br>2. Set Product A quantity to `1`. | **Outcome**: Accepted. Product A subtotal = `price * 1`. | Product A subtotal is `price * 1` | PASS
| **TC_BVA_03** | BVA | Cart Quantity = LB + 1 (2) | 1. Add product A using `Xem chi tiết`.<br>2. Set Product A quantity to `2`. | **Outcome**: Accepted. Product A subtotal = `price * 2`. | Product A subtotal = `price * 2`. | PASS

## AI Gap Analysis (BVA - FR-07)

After thorough examination, the AI (LLM) showed several limitations

- **Gap #1: AI using 'or' for TC_BVA_01 (Non-deterministic Outcome)** AI presented an expected outcome with one of the results, violating ISTQB'principle of determinism
- **Why:** LLMs are inherently probabilisitc. When dealing with cases that could have multiple interpretation about the expected outcome, AI has the tendency of stating every possible solution rather than committing to one result. This shows AI struggle in deterministic decision-making when lacking concrete information

## Feature: FR-14: Quản lý hồ sơ cá nhân (mobile)

BVA is executed on the ordered partition: **Phone Number Length** (Constraint: `10 <= length <= 11`).

### Ordered Partition Boundary Check

| Partition | LB | LB−1 | LB+1 | UB−1 | UB | UB+1 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Phone Number Length** | **10** | **9** | **11** | **10** | **11** | **12** |

- **Lower Boundary (LB = 10)**: Minimum valid length. Validates a standard 10-digit number.
- **LB - 1 (Length = 9)**: Invalid length. Verifies the input fails validation.
- **LB + 1 (Length = 11)**: Valid length. Validates a standard 11-digit number.
- **Upper Boundary (UB = 11)**: Maximum valid length.
- **UB + 1 (Length = 12)**: Invalid length. Verifies the input fails validation.

### BVA Test Cases

| Test Case ID | Technique | Boundary Covered | Inputs | Expected Outcome (Specification) | Actual Result | Verdict 
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC_BVA_01** | BVA | Phone Length = LB - 1 (9) | `phone`: "091234567" | **Failure**: Rejected by client validation. | **Failure** (but fails because it starts with 0, not due to length). | PASS 
| **TC_BVA_02** | BVA | Phone Length = LB (10) | `phone`: "0912345678" | **Success**: Profile is updated. | **Failure (Defect)**: Fails regex validation due to leading 0. | FAIL |
| **TC_BVA_03** | BVA | Phone Length = UB (11) | `phone`: "09123456789" | **Success**: Profile is updated. | **Failure (Defect)**: Fails regex validation (starts with 0 and exceeds length 10). | FAIL |
| **TC_BVA_04** | BVA | Phone Length = UB + 1 (12) | `phone`: "091234567890" | **Failure**: Rejected by client validation. | **Failure**: Rejected. | PASS |


