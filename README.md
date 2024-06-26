# Solar_Tracking_Simulink
----------------------------------------
Αυτόματο περιστροφικό φωτοβολταϊκό για μέγιστες επιδόσεις άντλησης ηλιακής ενέργειας  
--------------------------------------
Η αυτόματη περιστροφή φωτοβολταϊκών συστημάτων σύμφωνα με την ισχύ της εκπομπής ηλιακών ακτίνων πάνω στην γη φέρει τις μέγιστες αποδόσεις άντλησης ηλιακής ενέργειας από το φωτοβολταϊκο. Με στόχο να μεγιστοποιηθεί η ενέργεια άντλησης δημιουργήθηκε πρωτότυπο σύστημα ηλιακού πάνελ με διαστάσεις 24cm x 8cm που κινείται σύμφωνα με την ισχύ της εκπομπής του φωτός αριστερά και δεξιά της διάταξης. Τοποθετήθηκαν φωτοηλεκτρικές αντιστάσεις που μειώνουν την αντίσταση τους στην εκπομπή του φωτός και ορίζουν τις μοίρες που θα έχει ο σερβοκινητήρας που είναι τοποθετημένος στο κέντρο της κολώνας του φωτοβολταϊκού. Οι μοίρες που θα περιστραφεί ο σερβοκινητήρας καθορίζονται από υπολογισμούς που εκτελεί ο atmega328P πάνω στο αναπτυξιακό Arduino Uno. Δέχεται αναλογικές εισόδους τις εξόδους των 2 φωτοαντιστάσεων και παράγει ψηφιακή έξοδο pwm για να καθορίσει τις μοίρες για την περιστροφή του σερβοκινητήρα στην θέση όπου το φωτοβολταϊκό πάνελ έχει μέγιστη άντληση ενέργειας.

---------------------------------------------------------------
Simulink – Προσομοίωση 

Για την δημιουργία τέτοιου συστήματος και την επίτευξη του στόχου με μεθόδους trial-error και δοκιμής με διάφορα ηλεκτρονικά υλικά θα έπαιρνε μεγάλο χρονικό διάστημα και ταυτόχρονα μεγάλη δαπάνη χρημάτων. Επίσης η χρήση ενός μικροεπεξεργαστή απαιτεί τον προγραμματισμό του και αυτό καθυστερεί τις δοκιμές σε νέες μεθόδους κάθε φορά αφού πρέπει να προγραμματίζεται με νέο τρόπο. Οπότε χρησιμοποιήθηκε το λογισμικό Simulink της εταιρείας MathWorks όπου προσομοιώνεται το σύστημα σε μία πλατφόρμα σχεδίασης με blocks . Η πλατφόρμα εμπεριέχει βιβλιοθήκες για το Arduino Uno και blocks σχεδίασης που αφορούν το εκάστοτε αναπτυξιακό. Συγκεκριμένα εγκαθιστάτε η παρακάτω βιβλιοθήκη. 
![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/5ce74946-974c-4d47-9780-7cb38bd7c6ea)

--------------------------------------------------------------
Σχεδιασμός συστήματος 

Για την υλοποίηση του συστήματος δοκιμάστηκαν 3 τεχνικές εκ των οποίων οι 2 δεν δούλεψαν επειδή το Simulink δεν επιτρέπει ορισμένες λειτουργίες για το αναπτυξιακό Arduino Uno.
--------------------------------------------------------------
1η διάταξη

Έλεγχος θέσης του σερβοκινητήρα με  Fuzzy controller


![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/a258b66a-2eed-4ba9-8820-3209a757d23f)
![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/f3c32243-bcf7-475c-8c06-796cbf88b176)
Ο Fuzzy controller (ελεγκτής Fuzzy) είναι ένας τύπος ελεγκτή που βασίζεται στην λογική Fuzzy, μια ειδική μορφή λογικής που επιτρέπει την επεξεργασία και επιλογή ενέργειας σε περιβάλλοντα με ασαφείς συνθήκες. Στην πραγματικότητα, ο Fuzzy controller είναι ένας ελεγκτής που λειτουργεί με λογικές αποφάσεις, ενώ χρησιμοποιεί λογικούς κανόνες για την επεξεργασία και επιλογή μιας ενέργειας.

Είσοδοι :   Διαβάζει την κανονικοποιημένη διαφορά των φωτοαντιστάσεων και την εντάσσει σε ένα σύνολο με τρίγωνα , όπου ανάλογα την θέση έχει διαφορετικός βάρος ως προς την έξοδο.

![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/adc15a9d-c7cc-4f0e-926f-7b2d34e59b8a)

Έξοδοι :   Από 0 έως 180 για την θέση του σερβοκινητήρα.
![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/94081dc9-2e6c-4eea-8c6a-72df90412736)

Λογικοί κανόνες :
![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/8028d5cc-080e-4846-b69a-3f91248ab5a1)
Αν διαβάσει αρνητική τιμή σημαίνει ότι ο ήλιος εκπέμπει πιο δυνατά από τα αριστερά , άρα στον σερβοκινητήρα γράψε X μοίρες για αριστερά.

Αν διαβάσει μηδέν τιμή σημαίνει ότι ο ήλιος εκπέμπει το ίδιο και στις δύο φωτοαντιστάσεις , άρα στον σερβοκινητήρα μην στείλεις να αλλάξει θέση.

Αν διαβάσει θετική τιμή σημαίνει ότι ο ήλιος εκπέμπει πιο δυνατά από τα δεξιά, άρα στον σερβοκινητήρα γράψε Y μοίρες για δεξιά.

Το πόσο αριστερά θα στρίψει και το πόσο δεξιά το αποφασίζει ο ελεγκτής ανάλογα με την τιμή εισόδου και το βάρος που θα αποκτήσει μέσα στα σχεδιασμένα τρίγωνα.

Μετατροπή σε παλμό  ![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/fa6bd23a-b2ac-40a1-9631-80ed19160865)
Ο σερβοκινητήρας δέχεται παλμό μεγαλύτερου πλάτους από τον επιθυμητό για αυτό έγινε scale down με gain = 1/5 , τέλος το σήμα του fuzzy πηγαίνει σε ένα pwm generator όπου παράγει pwm . 


Παρατηρήσεις :  Ο Fuzzy δεν μπορεί να λειτουργήσει χωρίς την λειτουργία connected IO , επειδή καταλαμβάνει μεγάλο αποθηκευτικό χώρο που ξεπερνάει τα όρια μνήμης του Atmega328P. Όμως σε κατάσταση connected IO δεν μπορεί το σήμα pwm να ρυθμιστεί σωστά για να φτάσει ο σερβοκινητήρας τις εκάστοτε μοίρες που θέλει ο ασαφής ελεγκτής όταν υπάρχει Digital Out για το Arduino . Στην θέση αυτού χρησιμοποιήθηκε το Block servo write το οποίο δουλεύει (ύστερα από δοκιμές) μόνο όταν ένα σύστημα προσομοιώνεται σε Run on board , άρα δημιουργήθηκε ένα παράδοξο.
Βγάζει σωστή έξοδο ο Fuzzy αλλά δεν μπορεί να δημιουργηθεί σωστό PWM
και σε αντίθετη περίπτωση , υπάρχει σωστό PWM αλλά δεν μπορεί να χρησιμοποιηθεί ο Fuzzy.

--------------------------------------------------------------
2η διάταξη

Έλεγχος θέσης του σερβοκινητήρα με  λογικούς τελεστές

![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/ef05c594-4967-4562-bbbd-505c002bf8e5)
Το παραπάνω σύστημα ελέγχει κάθε φορά την διαφορά των τιμών των φωτοαντιστάσεων εάν είναι αρνητική τείνει στο 0 λόγο του Saturation block και αν είναι μέγιστη φτάνει έως 230. Δεν γινόταν η επεξεργασία αρνητικών τιμών και το Simulink έβγαζε μήνυμα σφάλματος Overflow.

![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/bbaaa6fd-cd17-442a-b8e5-f6b411d3dc14)

Στην συνέχεια επιλέγει λογικά με τελεστές ποια βαθμίδα θα ενεργοποιηθεί
Π.χ Εάν τελική τιμή μετά το saturation είναι ίση με 65 τότε η μεσαία βαθμίδα θα ενεργοποιηθεί , εάν έρθει τιμή 220 τότε η πρώτη βαθμίδα κ.ο.κ.
Ένα enabled Subsystem χρησιμοποιείται κάθε φορά.

Βαθμίδα πρώτη (η πιο πάνω στο σχήμα) εάν εντοπίσει τιμής μεγαλύτερη ή ίση του 190 τότε ενεργοποιεί το παρακάτω Block , δηλαδή ο σερβοκινητήρας περιστρέφεται 180 μοίρες που είναι δεξιά για το φωτοβολταϊκό.

![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/6d74108a-5110-40cf-8e9b-ca8cba702b1f)

Βαθμίδα δεύτερη (η μεσαία στο σχήμα) εάν εντοπίσει τιμής μεγαλύτερη ή ίση του 70 και ταυτόχρονα μικρότερη του 190 ενεργοποιεί το παρακάτω Block , δηλαδή ο σερβοκινητήρας περιστρέφεται 90 μοίρες που είναι το κέντρο στο τόξο περιστροφής για το φωτοβολταϊκό.

![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/f7ded4b8-c787-446b-896e-a32a4272bfe6)
Βαθμίδα τρίτη (η πιο κάτω στο σχήμα) εάν εντοπίσει τιμής μεγαλύτερη ή ίση του 0 και ταυτόχρονα μικρότερη του 70 ενεργοποιεί το παρακάτω Block , δηλαδή ο σερβοκινητήρας περιστρέφεται στις 0 μοίρες που είναι τέρμα αριστερά για το φωτοβολταϊκό.

![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/8ce6a4a1-b25c-49b2-8b2c-9072e80262ed)

Παρατηρήσεις :
Το Standard Servo Write δεν επιτρέπει στον χρήστη να προσθέσει περισσότερα από ένα Block με το ίδιο Pin . Άρα η λύση είναι να βραχυκυκλωθούν με καλώδια οι 3 έξοδοι σήματος σε ένα σερβοκινητήρα. Όταν δοκιμάστηκε η διάταξη το Servo ύστερα από την πρώτη του κίνηση σε κάποιες μοίρες στην επόμενη ξεκίναγε να πηγαίνει προς τυχαίες κατευθύνσεις χωρίς τέλος. Διαπιστώθηκε ότι το ψηφιακό κύκλωμα που έχει μέσα στο Servo κρατάει την τελευταία τιμή που είχε φτάσει το Servo για να δώσει feedback στο Arduino ότι όντως έφτασε. Όμως κάθε φορά που ένα νέο subsystem ενεργοποιούνταν το Arduino ήξερε ότι τα Pin 5 , 6 , 7 έχουν την δικιά τους διαφορετική θέση (σε μοίρες) ,στην πραγματικότητα όμως όλα είχαν την ίδια . Έτσι η διάταξη δεν δούλεψε , προτείνεται λύση η ύπαρξη ενός δυναμικού συστήματος με block που θα αλλάζει την τιμή του ανάλογα με το ποια είσοδο θα ενεργοποιηθεί ώστε 1) να μην χρειαστούν παραπάνω από 1 Pins για τον έλεγχο ενός σερβοκινητήρα και 2) για να μειώσει το υπολογιστικό κόστος.

--------------------------------------------------------------
3η διάταξη

Έλεγχος θέσης του σερβοκινητήρα ανάλογα με την διαφορά των εισόδων

![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/e70b988d-d089-4162-b542-49aeba364784)
Ο σερβοκινητήρας παίρνει ως όρισμα την διαφορά των 2 αναλογικών εισόδων και το μετατρέπει σε μοίρες. Σε περίπτωση που έρθει αρνητική τιμή πάει στην θέση 0 , σε περίπτωση που έρθει τιμή από 0-180 ακολουθεί σε μοίρες την εκάστοτε τιμή και τέλος αν υπερβεί η τιμή εξόδου πάνω από το 180 ο σερβοκινητήρας φτάνει την μέγιστη θέση 180 μοιρών.

Υλικά και Φυσική διάταξη

Χρησιμοποιήθηκαν 2 φωτοαντιστάσεις , 2 αντιστάσεις 1 KΩ και ένας σερβοκινητήρας. Οι αντιστάσεις 1 ΚΩ χρειάστηκαν για να ρυθμίσουν την έξοδο των φωτοαντιστάσεων στο φως όπου δοκιμαζόταν η διάταξη ( η επιλογή τους έγινε πειραματικά)

![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/2b27c1c4-9031-430f-9666-b8bcfb2040ca)

![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/db4ca212-c3c8-4ccd-ba8a-140574b316c9)


Παρατηρήσεις-Συμπεράσματα 

Το αναπτυξιακό Arduino έχει πολύ λίγες δυνατότητες για να εκτελέσει κάτι παραπάνω από έναν PID ελεγκτή και την δημιουργία ενός PWM σήματος , η μνήμη που του παρέχεται είναι λίγη και η υπολογιστική του δύναμη δεν ξεπερνάει μία πρόσθεση στην ALU. Το Simulink παρότι διαθέτει πολλά blocks για το Arduino,το arduino δεν μπορεί να υποστηρίξει μεγάλο σύνολο αυτών σε ένα σύστημα. Επίσης το project μπορεί να υλοποιηθεί και με άλλους τρόπους ακόμα πιο περίπλοκους όπως να δημιουργηθεί ακριβώς το PWM σήμα με pwm generator για να κινηθεί στις εκάστοτε μοίρες που προβλέπουν οι είσοδοι στο σύστημα , το οποίο δοκιμάστηκε αλλά δεν βρέθηκαν Blocks του Simulink που να υποστηρίζουν την αλλαγή του PWM όταν αλλάζει ένα σήμα εισόδου πέρα από τον Fuzzy ελεγκτή.
.
![image](https://github.com/eee-Andrew/Solar_Tracking_Simulink/assets/98215048/83e938dd-c4f4-46a7-ac6e-12df1ef12fb0)

--------------------------------------------------------------
 Ο πρώτος τρόπος λειτουργεί κανονικά εάν αλλάξει το αναπτυξιακό Arduino Uno με ένα STM32
 --------------------------------------------------------------

