# A02_Threading


import threading


class CounterThread(threading.Thread):

    threads = [3]
    frage = raw_input('Enter a word:')
    zfrage =  raw_input('entschluesseln ja nein')
    word = frage.lower()
    first = word[1]
    """     Verschluesselung     """
    def verschluessellung(h):
        h = 0

    """"
    Eingabe darf nicht leer sein
    und muss ein String sein

    Simple Verschluessellung
     First der Erste Buchstabe des Wortes


    """
    if len(frage) > 0 and frage.isalpha():
            words = "swag"
            new_word = 2*first +words + word + words+ 2*first
            print "Verschluesselt: "+new_word
    else:
            print 'Bitte geben sie was Ein'
    """"
        Eingabe darf nicht leer sein
        und muss ein String sein
     """
    def entschluessellung(h):
        h = 0
    if zfrage == 'ja' and zfrage.isalpha():
            print "Entschluesselt: "+frage.upper()
    else:
            print 'Bitte geben sie was Ein'


    """Hatte leider keine Zeit eine richtige Verschluesselung und entschluesselung mir auszudenken
        Die Idee mit Threading versteh ich leider nicht
    """
