import re

log = "2022-02-27 08:12:45 INFO This is a log message."

pattern = r"(\d{4}-\d{2}-\d{2}) (\d{2}:\d{2}:\d{2}) ([A-Z]+) (.+)"
match = re.match(pattern, log)

if match:
    print("Date:", match.group(1))
    print("Time:", match.group(2))
    print("Level:", match.group(3))
    print("Message:", match.group(4))
