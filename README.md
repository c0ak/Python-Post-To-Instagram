# About
The following scripts allow you to easily post images to Instagram using Python without any hassle.  
Copy and paste the two following scripts into you project and install the requirements and get yourself up and running in no time.  
Note: Windows Only!

# Requirements
```
pip install autoit
pip install selenium
pip install webdriver_manager
```

# The code
```
## driver.py

from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager


class Driver:

    def __init__(self):
        self.options = webdriver.ChromeOptions()
        self.driver = ""

    def capabilities(self):
        self.options.add_experimental_option("mobileEmulation", {"deviceName": "Pixel 2"})

    def setup(self):
        self.capabilities()
        self.driver = webdriver.Chrome(ChromeDriverManager().install(),
                                       desired_capabilities=self.options.to_capabilities())
        return self.driver
```
```
## actions.py

from time import sleep
from autoit import autoit
from driver import Driver


class Actions:

    def __init__(self):
        self.driver = Driver().setup()
        self.current_action = ""
        self.main_url = "https://www.instagram.com"
        self.driver.get(self.main_url)
        self.username = ""
        self.password = ""

    def login(self, username, password):
        self.username = username
        self.password = password
        sleep(4)
        login_button = self.driver.find_element_by_xpath("//button[contains(text(),'Log In')]")
        login_button.click()
        sleep(3)
        username_input = self.driver.find_element_by_xpath("//input[@name='username']")
        username_input.send_keys(self.username)
        password_input = self.driver.find_element_by_xpath("//input[@name='password']")
        password_input.send_keys(self.password)
        password_input.submit()

    def close_reactivated(self):
        try:
            sleep(2)
            not_now_btn = self.driver.find_element_by_xpath("//a[contains(text(),'Not Now')]")
            not_now_btn.click()
        except:
            pass

    def close_notification(self):
        try:
            sleep(2)
            close_noti_btn = self.driver.find_element_by_xpath("//button[contains(text(),'Not Now')]")
            close_noti_btn.click()
            sleep(2)
        except:
            pass

    def close_add_to_home(self):
        sleep(3)
        close_addHome_btn = self.driver.find_element_by_xpath("//button[contains(text(),'Cancel')]")
        close_addHome_btn.click()
        sleep(1)

    def new_post(self, image_path, caption):
        new_post_btn = self.driver.find_element_by_xpath("//div[@role='menuitem']").click()
        sleep(1.5)
        autoit.win_active("Open")
        sleep(2)
        autoit.control_send("Open", "Edit1", image_path)
        sleep(1.5)
        autoit.control_send("Open", "Edit1", "{ENTER}")
        sleep(2)
        next_btn = self.driver.find_element_by_xpath("//button[contains(text(),'Next')]").click()
        sleep(1.5)
        caption_field = self.driver.find_element_by_xpath("//textarea[@aria-label='Write a caption…']")
        caption_field.send_keys(caption)
        share_btn = self.driver.find_element_by_xpath("//button[contains(text(),'Share')]").click()
        sleep(25)
```

## Usage
```
from time import sleep
from actions import Actions

username = "xyz"
password = "xyz"
image_path = r"images\image.jpg"
caption = "Enter photo caption here" #Enter the caption

action = Actions()

action.login(username, password)
sleep(4)
action.close_reactivated()
action.close_notification()
action.close_add_to_home()
sleep(3)
action.close_notification()
action.new_post(image_path, caption)


action.driver.close()
```

Have fun
