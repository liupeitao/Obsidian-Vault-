### Selenium

简介： 自动化工具， 用代码操控浏览器

1. 启动浏览器：`$ driver = webdriver.Chrome()` (启动 Chrome 浏览器)
2. Firefox 浏览器：$ `driver = webdriver.Firefox()`
3. Safari 浏览器：$ `driver = webdriver.Safari()`
4. 打开网页：`$ driver.get("https://www.example.com")`
5. 查找元素：
   - 通过 ID 查找元素：`$ element = driver.find_element_by_id("element_id")`
   - 通过 CSS 选择器查找元素：`$ element = driver.find_element_by_css_selector("css_selector")`
   - 通过 XPath 查找元素：`$ element = driver.find_element_by_xpath("xpath_expression")`
   - 通过 class 定位元素：$ `element = driver.find_element_by_class_name("element_class")`
   - 通过标签名定位元素：$ `element = driver.find_element_by_tag_name("tag_name")`
6. 操作元素：
   - 点击元素：$ `element.click()`
   - 输入文本到输入框：$ `element.send_keys("text")`
   - 获取元素的文本内容：$ `text = element.text`
   - 获取元素的属性值：$ `value = element.get_attribute("attribute_name")`
   - 切换到 iframe：$ `driver.switch_to.frame("frame_name")`
   - 切换回默认的上下文：$ `driver.switch_to.default_content()`
7. 鼠标操作：
   - 鼠标移动到元素：`$ ActionChains(driver).move_to_element(element).perform()`
   - 右键点击元素：`$ ActionChains(driver).context_click(element).perform()`
8. 键盘操作：
   - 模拟按键输入：`$ element.send_keys(Keys.KEY_NAME)`
9. 切换窗口和帧：
   - 切换到新打开的窗口：`$ driver.switch_to.window(driver.window_handles[-1])`
   - 切换到 iframe：`$ driver.switch_to.frame("frame_name_or_id")`
10. 执行 JavaScript：`$ driver.execute_script("javascript_code")`
11. 屏幕截图（Screenshot）: $ `driver.save_screenshot("screenshot.png")`
12. 关闭浏览器（Close Browser）：$ `driver.quit()`
13. 等待（Waiting）：
- 隐式等待：$ `driver.implicitly_wait(10)`
- 显式等待：$ `WebDriverWait(driver, 10).until(EC.visibility_of_element_located((By.ID, "element_id")))`
