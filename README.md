FEATURE_FLAGS = {
    "ALERT_REPORTS": True,
    "THUMBNAILS": True,
}

# Required for local thumbnail generation
SCREENSHOT_LOCALLY = True
WEBDRIVER_TYPE = "chrome"
WEBDRIVER_OPTION_ARGS = [
    "--headless",
    "--disable-gpu",
    "--no-sandbox",
    "--disable-dev-shm-usage",
]
WEBDRIVER_BASEURL = "http://localhost:8088/"  # Use this if accessing Superset directly
