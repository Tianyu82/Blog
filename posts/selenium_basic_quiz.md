---
title: '[Selenium] Quiz - Basic Commands in Selenium (Python Version)'
date: '2024-10-30'
---

### Questions:
1. To work with a web page on Chrome, you first need to open a Chrome broswer. What object do you need to create first?

2. What are the codes to create such an object above?

3. Once a new web broswer object is created with an empty page, we want to navigate to https://www.google.com. 
   What are the codes to navigate to this web page?

4. After opening a new web page, it is advisable to check if the correct page is open.

   What are the codes respectively to assert the url and the title ('Google') of the current web page?

5. What are the codes to simulate navigating forward or backward of a web page?

6. What are the codes to simulate refreshing a web page?

7. After finish a testing, make sure to close the entire browser (along with all tabs). What's the command of doing that?

8. What if you don't want to close the entire browser? Instead, you only want to close the current tab (active tab). 

9. After you close a tab, the object reference points to the window that no longer exists.
   In this case, you cannot call any other WebDriver commands but except for one. What is that exception?

10. Opening multiple tabs in one web browser is a bit complicated. The best practice is to call a native JS code that directly interacts with the browser's JavaScript engine. What is the command to open a new tab in this way?

11. Now you have opened multiple tabs in one web broswer. You may need to go through all these tabs.
    Specifically, you need to get all open tabs (store them in a list), and iterate through the list. Write the command to iterate through all open tabs and switch to each tab.

12. Now let's practice a more difficult question. We have already opened a web browser. We want to open website 'A', then open up a new tab for website 'B', and then switches back to the tab of 'A'. You can assume the tabs are opened in order, so you can use the index of tab.

13. The questions above are about changing tabs within one web browser. Now, let's practice changing frames within one tab/web page. Changing frame is frequently required because one page may consist multiple frames (like regions). If you don't switch frames, then you won't be able to fetch the correct web elements. 
You are given that the ID of the frame is 'foo', what code will you use to switch to that frame?

14. If the frame has no ID or you want to use different strategy to find it, switching context can be done in two steps. First step, as mentioned earlier, is finding the frame as a web element (for example, the name of the frame is 'message'), and the second step is switching to the found frame. Please write the codes to achieve the function.

15. What codes would you use to switch back to the parent frame?

16. Selenium allows minimizing and maximizing the web browser and putting it into full screen mode. Please write the 3 commands respectively.

17. Taking screenshot is very important for doing automation testing. Screenshots can be taken in two different scopes: the entire browser page, or a single element in the browser page. Please write the commands to take screenshots for the entire web page and the specific element (named 'ele') respectively, and save the image to the address ('C:\\temp\\screenshot.png').

18. Please fetch the following HTML element by id:
    ```html
    <element id="unique_id">
    ```

19. Please fetch the following HTML element by class name:
    ```html
    <element class="class-name1">
    ```

20. Please fetch the following HTML element by tag name:
    ```html
    <h2>
    ```

21. Please fetch the following HTML element by link text:
    ```html
    <a href="next.html"> next page </a> 
    ```
    Note: please write two versions: one using the full link text, and the other using partial link text

22. Please fetch the third input field (the submit button) by using XPath, try using full path and relative path:
    ```html
    <html>
        <body>
            <form id="sample_form1">
                <input name="text1" type="text" />
                <input name="text2" type="text" />
                <input name="submit_button" type="submit" value="Enter" />
            </form>
        </body>
    </html>
    ```

23. Please fetch the "Some Latin nonsense" paragraph using CSS Selector. (hint: use the unqiue class name)
    ```html
    <html>
        <body>
            <p class="paragraph">Some Latin nonsense.</p>
            <p class="highlight">Please ignore.</p>
        </body>
    </html>
    ```

24. What are the commands to set the texts of the input field named "element" to 'XYZ'?
    (Note: make sure you clear the input field first)

25. Imagine you have located a radio button named 'option1'. You want to click it, then verify that the radio button is actually selected. Please write the commands.

26. Now, let's manipulate on checkboxes. While we click on check boxes, they must be treated differently than other clickable controls. If you click on a radio button multiple times, it remains selected. However, every time you click on a checkbox, it toggles the selected state. Therefore, it is important to understand the state that we want to achieve when manipulating a checkbox. We can write a function which will take the checkbox element and the desired state and perform the right action no matter the current state the checkbox is in. Please finish the following function:
- def set_check_box(element, want_checked):
-    click (select) the element if you want to select it and it is not selected
-    click (unselect) the element if you want to unselect it and it is selected

27. Now, let's work on dropdown lists. Imagine we have located and clicked on the dropdown list element named "dropList":
    ```html
    <select id="dropdown">
        <option value="1">Apple</option>
        <option value="2">Banana</option>
    </select>
    ```
    Please write the following commands:
    - (1) Selects the option with value="1" (You won't see '1' on the list, instead you will actually select "Apple")
    - (2) Selects the option that displays "Apple"
    - (3) Selects the first option on the list (0-indexed)
    - (4) Imagine you have selected multiple options, deselect all selected options
    - (5) Imagine you have selected multiple options, deselects the second option (remember the list is 0-indexed)
    - (6) Imagine you have selected multiple options, deselects the option that displays "Banana"
    - (7) Imagine you have selected multiple options, deselects the option with value="2"
    - (8) Get all selected options (no matter single-selected list or multi-selected list), and store in a list variable named "selected_options"
    - (9) Get the text of the first selected option (no matter single-selected list or multi-selected list), and store in a variable named "first_selected"

28. The last question is about handling dialog. There are three types of dialogs: alert, confirm, and prompt. 
    Imagine you see an alert poping up. You need to handle the followings using Selenium codes:
    - (1) switch context to the alert object
    - (2) retrieve the text from the alert
    - (3) accept the alert ( click ok)
    - (4) The confirm and promp dialog are pretty much the same as the alert dialog, except for that in step 3, you can choose to dismiss the dialog. What is the command to dismiss a dialog element named "dialog"?


#### Answers:
1. A WebDriver object for Chrome.
2. 
   ```python
   from selenium import webdriver
   driver = webdriver.Chrome()
   ```
3. 
   ```python
   driver.get('https://www.google.com')
   ```
4. 
   ```python
   assert driver.current_url == 'https://www.google.com'
   assert driver.title == 'Google'
   ```
5. 
   ```python
   driver.forward() 
   driver.back()
   ```
6. 
   ```python
   driver.refresh()
   ```
7. 
   ```python
   driver.quit()
   ```
8. 
   ```python
   driver.close()
   ```
9. 
   ```python
   driver.switch_to.window()
   ```
10. 
   ```python
   driver.execute_script("window.open('');")
   ```
11. 
   ```python
   for handle in driver.window_handles:
       driver.switch_to.window(handle)
   ```
12. 
   ```python
   driver.get('A')
   driver.execute_script("window.open('');")
   driver.switch_to.window(driver.window_handles[1])
   driver.get('B')
   driver.switch_to.window(driver.window_handles[0])
   ```
13. 
   ```python
   driver.switch_to.frame('foo')
   ```
14. 
   ```python
   message_frame = driver.find_element_by_name('message')
   driver.switch_to.frame(message_frame)
   ```
15. 
   ```python
   driver.switch_to.parent_frame()
   ```
16. 
   ```python
   driver.maximize_window()
   driver.minimize_window()
   driver.fullscreen_window()
   ```
17. 
   ```python
   driver.get_screenshot_as_file('C:\temp\screenshot.png')
   ele.screenshot('C:\temp\screenshot.png')
   ```
18. 
   ```python
   ele = driver.find_element_by_id('unique_id')
   ```
19. 
   ```python
   ele = driver.find_element_by_class_name('class-name1')
   ```
20. 
   ```python
   ele = driver.find_element_by_tag_name('h2')
   ```
21. 
   ```python
   ele = driver.find_element_by_link_text('next page')
   ele = driver.find_element_by_partial_link_text('next pa')
   ```
22. 
   ```python
   # Full path:
   ele = driver.find_element_by_xpath('/html/body/form/input[3]')
   # Relative path:
   ele = driver.find_element_by_xpath('//form[@id="sample_form1"]/input[3]')
   ```
23. 
   ```python
   ele = driver.find_element_by_css_selector('p.paragraph')
   ```
24. 
   ```python
   element.clear()
   element.send_keys('XYZ')
   ```
25. 
   ```python
   option1.click()
   assert option1.is_selected()
   ```
26. 
   ```python
   def set_check_box(element, want_checked):
       if (want_checked and not element.is_selected()) or (not want_checked and element.is_selected()):
           element.click()
   ```
27. 
   ```python
   dropList.select_by_value('1')
   dropList.select_by_visible_text('Apple')
   dropList.select_by_index(0)
   dropList.deselect_all()
   dropList.deselect_by_index(1)
   dropList.deselect_by_visible_text('Banana')
   dropList.deselect_by_value('2')
   selected_options = dropList.all_selected_options
   first_selected = dropList.first_selected_option.text
   ```
28. 
   ```python
   alert = driver.switch_to.alert
   msgText = alert.text
   alert.accept()  # click OK
   dialog.dismiss()  # Click Cancel
   ```
