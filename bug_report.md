# Bug Report

## FR-01
1. Lacking Password confirmation prompt
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR01-01                                                                                           |
| **Severity**           | High                                                                                                 |
| **Priority**           | High                                                                                                 |
| **Summary**            | The account registration lacking the password confirmation form |
| **Steps to Reproduce** | 1. Go to frontend-web <br>2. Click "Đăng ký" <br>    |
| **Expected**           | The password confirmation form should exist                                                    |
| **Actual**             | The password confirmation form does not exist                                                  |

Bug Link: https://github.com/npmthu/web-automation-testing/issues/64

2. Proper password gets rejected due to a flawed Regex
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR01-02                                                                                           |
| **Severity**           | Critical                                                                                              |
| **Priority**           | High                                                                                                  |
| **Summary**            | A password that qualifies the specification gets rejected |
| **Steps to Reproduce** | 1. Go to frontend-web <br>2. Click "Đăng ký" <br>  3. Fill out the sign up form like this in the following order:  "Nguyễn Văn A", "new_user@eshop.com", "Password123!|
| **Expected**           | The user should be registered, and the user gets directed back to the sign in tab                                                    |
| **Actual**             | An error message pops out: "Mật khẩu quá yếu! Phải dài tối thiểu 8 ký tự, gồm chữ hoa, chữ thường, số và KÝ TỰ ĐẶC BIỆT."                                                 |
Bug Link: https://github.com/npmthu/web-automation-testing/issues/65

3. Invalid email format gets validated

| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR01-03                                                                                           |
| **Severity**           | Critical                                                                                              |
| **Priority**           | High                                                                                                  |
| **Summary**            | Invalid email format gets validated due to lack of validation, and the field being <text> rather than <email> |
| **Steps to Reproduce** | 1. Go to frontend-web <br>2. Click "Đăng ký" <br> 3. Fill in the information in the following order: "Nguyễn Văn A", [any invalid email format], "Password 123" (note: you must use "Password 123" as your password to make the bug happen)   |
| **Expected**           | An error message about invalid Email format                                                   |
| **Actual**             | The account got registered, and the user gets redirected to the Sign in page
Bug Link: https://github.com/npmthu/web-automation-testing/issues/66

4. Duplicate email adress gets permitted
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR01-04                                                                                           |
| **Severity**           | Critical                                                                                              |
| **Priority**           | High                                                                                                  |
| **Summary**            | Invalid email format gets validated due to lack of validation, and the field being <text> rather than <email> |
| **Steps to Reproduce** | 1. Go to frontend-web <br>2. Click "Đăng ký" <br> 3. Fill in the information in the following order: "Nguyễn Văn A", [any existing email, such as "test@eshop.com"], "Password 123" (note: you must use "Password 123" as your password to make the bug happen)  |
| **Expected**           | An error message about invalid the Email being already registered                                                |
| **Actual**             | The account got registered, and the user gets redirected to the Sign in page
Bug Link: https://github.com/npmthu/web-automation-testing/issues/67

## FR-07:  
1. Duplicate product lines
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR07-01                                                                                           |
| **Severity**           | Major                                                                                                 |
| **Priority**           | High                                                                                                  |
| **Summary**            | Product page showing duplicate products rather than adding |
| **Steps to Reproduce** | 1. Go to frontend-web <br>2. Click "Thêm vào giỏ" multiple time for a product <br>  3. Go to "Giỏ hàng  |
| **Expected**           | Product showing the updated value rather than being duplicated                                          |
| **Actual**             | The product being duplicated                                                  |
Bug link: https://github.com/npmthu/web-automation-testing/issues/68

2.Negative cart value permitted
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR07-02                                                                                           |
| **Found at**      | TC_DOM_VAL_01                                            |
| **Severity**           | Major                                                                                                 |
| **Priority**           | High                                                                                                  |
| **Summary**            | Frontend validates negative cart quantity |
| **Steps to Reproduce** | 1. Go to frontend-web <br>2. Click "Xem chi tiết" <br>  3. At the value section, put a negative value. <br>  4. Go to "Giỏ hàng   |
| **Expected**           | System rejecting the value                                                    |
| **Actual**             | The value being permitted                                                  |
Bug link: https://github.com/npmthu/web-automation-testing/issues/69

3. Float value truncated
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR07-03                                                                                           |
| **Severity**           | Major                                                                                                 |
| **Priority**           | High                                                                                                  |
| **Summary**            | Frontend validates truncate a float product quantity rather than rejecting it |
| **Steps to Reproduce** | 1. Go to frontend-web <br>2. Click "Xem chi tiết" <br>  3. At the value section, put a float value like 2.5. <br>  4. Go to "Giỏ hàng   |
| **Expected**           | System rejecting the value                                                 |
| **Actual**             | The value being permitted                                                  |
Bug link: https://github.com/npmthu/web-automation-testing/issues/70

4.Empty value permitted:
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR07-04                                                                                           |
| **Severity**           | Major                                                                                                 |
| **Priority**           | High                                                                                                  |
| **Summary**            | Frontend validates empty cart quantity |
| **Steps to Reproduce** | 1. Go to frontend-web <br>2. Click "Xem chi tiết" <br>  3. At the value section, put 0. <br>  4. Go to "Giỏ hàng   |
| **Expected**           | System rejecting the value                                                    |
| **Actual**             | The value being permitted                                                  |
Bug link: https://github.com/npmthu/web-automation-testing/issues/71

5. Non numeric value permitted
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR07-05                                                                                           |
| **Severity**           | Major                                                                                                 |
| **Priority**           | High                                                                                                  |
| **Summary**            | Frontend validates non-numeric value quantity |
| **Steps to Reproduce** | 1. Go to frontend-web <br>2. Click "Xem chi tiết" <br>  3. At the value section, put blank. <br>  4. Go to "Giỏ hàng   |
| **Expected**           | System rejecting the value                                                 |
| **Actual**             | The value being permitted                                                  |
Bug link: https://github.com/npmthu/web-automation-testing/issues/72

6. No increment button on frontend cart
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR07-06                                                                                           |
| **Severity**           | Major                                                                                                 |
| **Priority**           | High                                                                                                  |
| **Summary**            | Increment button does not exist in the cart |
| **Steps to Reproduce** | 1. Go to frontend-web <br>2 . Go to "Giỏ hàng"   |
| **Expected**           | An increment button should exist                                                    |
| **Actual**             | The button does not exist                  |
Bug link: https://github.com/npmthu/web-automation-testing/issues/73

## FR-13
1. Inaccurate caluclation for total amount
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR13-01                                                                                           |
| **Severity**           | Critical                                                                                              |
| **Priority**           | High                                                                                                  |
| **Summary**            | The revenue multiply the total sum |
| **Steps to Reproduce** | 1. Add some product into cart <br>2. Go to Admin dashboard <br>  3. Mark some orders as delivered <br>  4. Check the actual total amount |
| **Expected**           | The total amount is being calculated properly                                                    |
| **Actual**             | The total amount is doubled (for example: the result showed should be 30.000.000, instead of 60.000.000)                                                 |
Bug link: https://github.com/npmthu/web-automation-testing/issues/74

2. Frontend validates negative amount
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR13-02                                                                                           |
| **Severity**           | Critical                                                                                              |
| **Priority**           | High                                                                                                  |
| **Summary**            | Negative amount is permitted |
| **Steps to Reproduce** | 1. Add an order with negative quantity <br>2. Go to Admin dashboard <br>  3. Mark some orders as delivered <br>  4. Check the actual total amount |
| **Expected**           | System rejects the result                                                    |
| **Actual**             | The total amount is calculated normally                                               |
Bug link: https://github.com/npmthu/web-automation-testing/issues/75

3. Frontend validates float amount
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR13-03                                                                                           |
| **Severity**           | Critical                                                                                              |
| **Priority**           | High                                                                                                  |
| **Summary**            | Float amount is permitted |
| **Steps to Reproduce** | 1. Add an order with a float price <br>2. Go to Admin dashboard <br>  3. Mark some orders as delivered <br>  4. Check the actual total amount |
| **Expected**           | System rejects the result                                                    |
| **Actual**             | The total amount is calculated normally                                               |
Bug link: https://github.com/npmthu/web-automation-testing/issues/76

4. Frontend validates non-numerical amount
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR13-04                                                                                           |
| **Severity**           | Critical                                                                                              |
| **Priority**           | High                                                                                                  |
| **Summary**            | Negative amount is permitted |
| **Steps to Reproduce** | 1. Add an order with NaN quantity <br>2. Go to Admin dashboard <br>  3. Mark some orders as delivered <br>  4. Check the actual total amount |
| **Expected**           | System rejects the result                                           |
| **Actual**             | The actual result is 0                                              |
Bug link: https://github.com/npmthu/web-automation-testing/issues/77

## FR-04 (Mobile)
1. Flawed phone number regex
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR04-01                                                                                           |
| **Severity**           | Critical                                                                                              |
| **Priority**           | High                                                                                                  |
| **Summary**            | Number starts with 0 are rejected while the one not starting with 0 gets validated |
| **Steps to Reproduce** | 1. Login to an account <br> 2.  Go to the user edit page <br> 3. Update the user with the following information: Name: Nguyen Van A. Phone Number: 0912345678 (valid)/912345678 (invalid)|
| **Expected**           | System permits "0912345678" while not "912345678"                                                  |
| **Actual**             | "0912345678" gets rejected while "912345678" gets validated                                        |
Bug link: https://github.com/npmthu/web-automation-testing/issues/78

2. API Privilege escalation
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR04-02                                                                                           |
| **Severity**           | Critical                                                                                              |
| **Priority**           | High                                                                                                  |
| **Summary**            | User can get a privilege by using API response |
| **Steps to Reproduce** | *Direct API body update:*<br>`name`: "Nguyen Van A"<br>`phone`: "0912345678"<br>`role`: "admin" |
| **Expected**           | API ignores role update                                                  |
| **Actual**             | Success message: profile returned, with the account's role being escalated                                             |
Bug link: https://github.com/npmthu/web-automation-testing/issues/79

3. Wrong log-out button
| Field                  | Detail                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------- |
| **Bug ID**             | BUG-FR04-03                                                                                           |
| **Severity**           | Minor                                                                                                 |
| **Priority**           | Low                                                                                                  |
| **Summary**            | Log out button should be "Đăng xuất" instead of "Thoát |
| **Steps to Reproduce** | 1. Login to an account <br> 2.  Go to the user edit page|
| **Expected**           | Log out button should be "Đăng xuất"                                           |
| **Actual**             | Log out button is "Thoát" instead                                             |
Bug link: https://github.com/npmthu/web-automation-testing/issues/80