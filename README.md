import threading

import time

from random import randint





"""

Dictionary with all characters and index

"""

alphabet = {1: "a", 2: "b", 3: "c", 4: "d", 5: "e", 6: "f", 7: "g", 8: "h", 9: "i", 10: "j", 11: "k", 12: "l", 13: "m",

            14: "n", 15: "o", 16: "p", 17: "q", 18: "r", 19: "s", 20: "t", 21: "u", 22: "v", 23: "w", 24: "x", 25: "y",

            26: "z"}



"""

Dictionary for the encryption

"""

key = {}





def generate_dictionary():

    """

    Generate a random key for the encryption with the help of the alphabet dictionary

    """

    for c in alphabet:

        while True:

            rand = randint(1, 26)

            same = False

            for i in key:

                if alphabet[rand] == key[i]:

                    same = True

            if same == False:

                key[alphabet[c]] = alphabet[rand]

                break

generate_dictionary();





def input_choose(number):

    """

    input_choose handle the number input of the user

    :param number: input from the user

    :return:

    """

    if number == 0:

        while True:

            print "Wollen Sie das Programm wirklich beenden, j: Ja, n: Nein"

            choose = raw_input()

            if choose.lower() == "j":

                quit()

            elif choose.lower() == "n":

                input_user()

            else:

                print "Bitte nur j fuer Ja oder n fuer Nein eingeben."

    if number == 1:

        encrypt_input()

    if number == 2:

        decrypt_input()





def encrypt_input():

    """

    Handles the user input for the message to encrypt and the count of threads

    :return:

    """

    text = ""

    threadcount = 0

    while True:

        print "Nachricht zum Verschluesseln eingeben"

        text = raw_input()

        if len(text) > 0:

            text = text.lower()

            break

        print "Bitte eine Nachricht eingeben."

    while True:

        print "Wie viele Threads sollen verwendet werden"

        choose = raw_input()

        if choose.isdigit():

            if len(text) / int(choose) < 1:

                print "Zu viele Threads fuer so einen kurzen Text, max " + str(len(text))

            else:

                threadcount = int(choose)

                break

        else:

            print "Bitte nur Zahlen eingeben"



    threads = []

    parts, part_size = len(text), len(text) / threadcount

    texts = [text[i:i + part_size] for i in range(0, parts, part_size)]



    for i in range(1, threadcount + 1):

        thread = EncryptionThread(i, texts[i - 1])

        threads += [thread]

        thread.start()



    encryption = ""

    for x in threads:

        x.join()

        encryption += x.result

    print "Die verschluesselte Nachricht lautet: " + encryption





class EncryptionThread(threading.Thread):

    """

    Thread encryp the text, which the thread get via parameter in init

    """

    __anzahl = 0



    def __init__(self, thread_number, text):

        threading.Thread.__init__(self)

        self.thread_number = thread_number

        self.text = text

        self.result = ""

        EncryptionThread.__anzahl += 1



    def run(self):

        for c in self.text:

            if c not in key:

                self.result += c

            else:

                self.result += key[c];





def decrypt_input():

    """

    Handles the user input for the message to decrypt and the count of threads

    :return:

    """

    text = ""

    threadcount = 0

    while True:

        print "Nachricht zum Entschluessel eingeben (muss mit den Schluessel der derzeitigen Session" \

              " erzeugt worden sein)"

        text = raw_input()

        if len(text) > 0:

            text = text.lower()

            break

        print "Bitte die Nachricht eingeben."

    while True:

        print "Wie viele Threads sollen verwendet werden"

        choose = raw_input()

        if choose.isdigit():

            if len(text) / int(choose) < 1:

                print "Zu viele Threads fuer so einen kurzen Text, max " + str(len(text))

            else:

                threadcount = int(choose)

                break

        else:

            print "Bitte nur Zahlen eingeben"



    threads = []

    parts, part_size = len(text), len(text) / threadcount

    texts = [text[i:i + part_size] for i in range(0, parts, part_size)]



    for i in range(1, threadcount + 1):

        thread = DecryptionThread(i, texts[i - 1])

        threads += [thread]

        thread.start()



    decryption = ""

    for x in threads:

        x.join()

        decryption += x.result

    print "Die entschluesselte Nachricht lautet: " + decryption





class DecryptionThread(threading.Thread):

    """

    Thread decryp the text, which the thread get via parameter in init

    """

    __anzahl = 0



    def __init__(self, thread_number, text):

        threading.Thread.__init__(self)

        self.thread_number = thread_number

        self.text = text

        self.result = ""

        DecryptionThread.__anzahl += 1



    def run(self):

        for c in self.text:

            if c not in key:

                self.result += c

            else:

                for key2, value in key.iteritems():

                    if value == c:

                        self.result += key2





def input_user():

    """

    Handle the user input for encryption, decryption, new key and exit

    :return:

    """

    while True:

        print "Was moechten Sie tun? 1: Verschluesseln, 2: Entschluesseln, 0: Exit"

        choose = raw_input()

        if choose.isdigit():

            input_choose(int(choose))

        else:

            print "Falsche Eingabe, bitte nur 1, 2 oder 0 eingeben."

input_user()
