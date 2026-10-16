HOW TO EDIT THIS FILE
======================
This file is the chatbot's knowledge base. The "Archive Assistant" on the
book's website fetches this file fresh on every page load, splits it into
sections, and searches those sections to answer visitor questions — a small
retrieval system (RAG) running entirely in the browser, no server required.

You can edit this file independently in the GitHub repo and the chatbot will
pick up your changes automatically next time someone loads the page. No code
changes needed. Rules:

- Start every section with "## " followed by a short title. Everything until
  the next "## " belongs to that section.
- Keep each section focused on one topic — a few sentences to a short
  paragraph works best. Very long sections dilute search relevance.
- Optional: put a line that starts with "action:" right after the heading to
  attach a button to that answer. Supported values:
    action: amazon          -> adds a "Buy on Amazon" button
    action: sample           -> adds a "Read the Foreword" button
    action: chapter:c1       -> adds a "Read this chapter" button (only
                                 works for chapter ids that are unlocked
                                 free previews: c2, c3, c4, or fw)
    action: review           -> adds a "Write a review" button
    action: reviews          -> adds a "See reader reviews" button
    action: rate             -> adds a "Rate this book" button
- Add as many new "## " sections as you like — new chapters, updated pricing,
  reviews, events, whatever. There's no limit the chatbot enforces.
- This file is plain text / Markdown-flavored, but it is never rendered as a
  page — it's only read by the chatbot, so formatting stays simple.

CONVERSATION SECTIONS (small talk, opening message, "not found" reply)
-----------------------------------------------------------------------
Sections whose first line is "type:" are NOT searched for book answers. They
control how the assistant chats. Three kinds:

  type: smalltalk   a reply to everyday chat (hello, thanks, how are you...)
      match:   trigger phrases separated by |  (lowercase, no punctuation)
      mode:    exact     the whole message must equal one of the phrases
               contains  a short message (7 words or fewer) containing a
                         phrase as whole words. Default if mode is omitted.
      reply:   one possible answer. Write several "reply:" lines and the
               assistant picks one at random each time. One line each.
      action:  optional button, comma separated: amazon, sample, chapter:c1,
               reviews (jump to reviews), review (open the review form),
               rate (jump to the star rating)
      capture: name   (optional) remembers the single word after the phrase
                      as the visitor's name, e.g. "my name is Amina"

  type: opening     the first message shown when the chat loads.
                    Write one or more "reply:" lines.
  type: fallback    what the assistant says when nothing in this file
                    matches a question. One or more "reply:" lines.
                    (Search and Amazon buttons are added automatically.)

  type: synonyms    lets the assistant treat different words as the same
                    topic. One group per line:  word: other, words, here
                    (e.g.  cost: price, fee, charge). Typing "how much is the
                    fee" then finds the section that says "price".

Placeholders you can use inside any reply:
  {greeting}  Good morning / Good afternoon / Good evening
  {name}      ", Amina" when the visitor gave a name, otherwise nothing
              (so write "Goodbye{name}!" and it becomes "Goodbye, Amina!")
  {title}     the book title

Rules are checked from top to bottom and the FIRST match wins, so keep
specific rules above general ones. Reload the page after editing.

Everything below this line and above the first "## " is ignored by the
chatbot — use this space for your own notes if useful.


## Small talk: Introductions
type: smalltalk
mode: exact
capture: name
match: my name is | my name's | call me | i am called | this is | hi my name is | hello my name is | hey my name is
reply: Nice to meet you{name}! What would you like to know about the book?
reply: Great to meet you{name}. Ask me anything about the book, from the chapters to the author.

## Small talk: Greeting
type: smalltalk
mode: exact
match: hi | hello | hey | hiya | yo | howdy | greetings | good morning | good afternoon | good evening | good day | sup | hola | salam | hi there | hello there | hey there | hello again | hi again | hello everyone | hi all
reply: {greeting}{name}! Welcome. I can tell you about the book, its chapters, the author, or what you can read for free. What are you curious about?
reply: Hello{name}! Glad you stopped by. Ask me anything about {title}.
reply: Hi{name}! How can I help you with the book today?

## Small talk: How are you
type: smalltalk
mode: contains
match: how are you | how r u | how are u | how's it going | hows it going | how is it going | how do you do | how have you been | what's up | whats up | you good | are you ok | are you okay
reply: I'm doing well, thank you for asking! Ready to talk about healthcare cybersecurity whenever you are. How are you doing today?
reply: Doing great, thanks! It's a good day to talk about protecting patient data. How about you?
reply: All good on my side. How can I help you today?

## Small talk: Visitor is fine
type: smalltalk
mode: exact
match: i'm good | im good | i am good | good | fine | i'm fine | im fine | i am fine | well | i'm well | i am well | alright | i'm alright | not bad | pretty good | all good | excellent | wonderful | doing good | doing well | doing great | i'm doing well | i'm doing great | good thanks | fine thanks | good and you | fine and you | great thanks | great
reply: Glad to hear it! What would you like to explore: the chapters, the author, or the free previews?
reply: Wonderful. Is there something about the book I can help you find?

## Small talk: Thanks
type: smalltalk
mode: exact
match: thank you | thanks | thx | cheers | many thanks | much obliged | i appreciate it | appreciate it | thank you so much | thanks so much | thank you very much | thanks a lot | thanks again | thank you again | ok thanks | okay thanks | great thanks | cool thanks | awesome thanks | thanks mate | thanks for the help | thank you for the help | thanks for that | thank you for that | thanks for the info | thank you for the information
reply: You're very welcome{name}! Anything else you would like to know?
reply: Happy to help! Let me know if there is anything else about the book.

## Small talk: Goodbye
type: smalltalk
mode: exact
match: bye | goodbye | good bye | see you | see ya | cya | later | good night | take care | that's all | thats all | i'm done | im done | no more questions | bye for now | see you later | see you soon | goodbye for now
action: amazon
reply: Goodbye{name}! If you'd like the full book, it's on Amazon. Come back anytime.
reply: Take care{name}! Enjoy the free chapters, and thanks for visiting.

## Small talk: Who are you
type: smalltalk
mode: contains
match: who are you | what are you | your name | are you a bot | are you a robot | are you ai | are you human | are you real | are you a person | are you chatgpt | are you claude
reply: I'm the Archive Assistant for this book. I'm not a person: I search the book's knowledge file and answer from it, so everything I tell you comes from the author's own material. If I can't find something, I'll say so and point you to a web search or the full book.

## Small talk: What can you do
type: smalltalk
mode: exact
match: help | help me | can you help | can you help me | what can you do | how do you work | how can you help | how can you help me | what do you know | what should i ask | what can i ask | what can i ask you
reply: I can answer questions about the book: what it covers, any of its 13 chapters, the author, the price and formats, and which chapters you can read free here. I can also compare chapters, open a free chapter for you, tell you what readers say, and walk you through rating or reviewing the book. Try "compare chapter 5 and 6", "open chapter 7" or "what do readers say?"

## Small talk: Compliments
type: smalltalk
mode: contains
match: you are great | you are awesome | you are amazing | you are helpful | you are smart | you are good | you're great | you're awesome | you're amazing | you're helpful | you're smart | youre great | youre awesome | this is great | this is awesome | that's great | thats great | that was helpful | well done | good job | great job | nice job
reply: Thank you, that means a lot! Is there anything else I can look up for you?
reply: Glad that helped! Ask away if there's more you'd like to know.

## Small talk: Acknowledgements
type: smalltalk
mode: exact
match: ok | okay | k | alright then | sure | yes | yeah | yep | yup | no | nope | nah | hmm | hm | i see | got it | understood | right | cool | nice | awesome | amazing | perfect | brilliant
reply: Great. What else would you like to know about the book?
reply: Alright. I'm here if you have more questions.

## Small talk: Apologies
type: smalltalk
mode: contains
match: sorry | my bad | oops | apologies
reply: No need to apologise at all. What can I help you with?

## Quick answer: Price
type: smalltalk
mode: contains
match: how much is it | how much is the book | how much does it cost | how much does the book cost | how much do you charge | what is the price | what's the price | whats the price | what does it cost | price | cost
action: amazon
reply: The book costs $20.00 and is available from Amazon. (If you change the price, update it in the "Price, cost and where to buy" section below too.)

## Synonyms
type: synonyms
cost: price, charge, fee, pay, expensive, cheap, afford
buy: purchase, order, shop, amazon
author: writer, wrote, written, creator, samuel, nambili
free: sample, preview, complimentary, unlocked
healthcare: health care, medical, clinical, hospital, hospitals, patient, patients
ransomware: extortion, ransom, wannacry, conti, lockbit, doppelpaymer
breach: leak, leaked, hack, hacked, hacking, stolen
privacy: confidentiality, confidential, hipaa, gdpr, phi, protected
devices: device, iot, iomt, wearable, wearables, infusion, pump, pumps, implant, implants
ehr: records, record, emr, electronic
offensive: pentest, pentesting, penetration, redteam, ethical, hacker
defensive: defence, defense, defend, blue, protect, protection
ai: artificial intelligence, machine learning, shield, sword
threat: risk, danger, attack, hazard
insider: employee, staff, internal
nation: state, apt, government, espionage
cia: confidentiality, integrity, availability, triad
pages: length, size
format: paperback, print, printed, book

## How to rate the book
action: rate
You can rate the book with the five stars beside "What's inside". You do not need an account to rate, and each device can rate once.

## How to write a review
action: review
Scroll to Reader reviews and choose Write a review. Sign in with LinkedIn or create an account with your email, confirm your email address, create a quick profile with your name, headline and optional photo, then write your review. You can edit or delete it later.

## Reviewer accounts, privacy and email
action: review
Reviewer profiles live only on this book's website, not on GitHub. Your name, photo, headline and review are public, and your email address is never shown. A confirmed email is required so that every review comes from a real person.

## Where the book covers ransomware and attackers
Chapter 3 (Who Hack Healthcare and Why) covers ransomware-as-a-service, hacker types, nation-state actors, insider threats and why healthcare is a preferred target. Chapter 4 (Cyber Threats Targeting Healthcare Systems) walks through the anatomy of a modern ransomware attack, phishing and supply-chain attacks. Chapter 12 draws lessons from real case studies.

## Where the book covers medical devices and IoT
Chapter 5 (Security Risks in Medical Devices and IoTs) is devoted to connected medical devices. The Introduction and Chapter 1 explain how EHRs connect to infusion pumps, ventilators, wearables and other Internet of Medical Things (IoMT) devices.

## Where the book covers patient privacy and regulation
Chapter 6 (Patient Data Privacy and Protection) covers patient data privacy and the rules that govern medical data. Chapter 2 explains confidentiality, including what counts as Protected Health Information, and the Introduction discusses HIPAA and GDPR.

## Where the book covers offensive and defensive security
Part III covers both sides: Chapter 7 (Offensive Security in Healthcare) and Chapter 8 (Defensive Cybersecurity Strategies). Chapter 3 also explains white hat, black hat and gray hat hackers.

## Where the book covers AI
AI appears throughout. The Foreword calls AI both a shield and a sword; Chapter 1 introduces AI in healthcare and cybersecurity; Chapter 9 covers Artificial Intelligence as a Shield and Chapter 10 covers Artificial Intelligence as a Sword.

## Where the book covers case studies and the future
Chapter 12 (Lessons from the Case Studies) distils lessons from real incidents, and Chapter 13 (The Future of Cybersecurity in Healthcare) looks ahead. The Foreword mentions WannaCry and the NHS, the Düsseldorf University Hospital attack, the Irish HSE attack and a 2024 attack on Namibia's Ministry of Health.

## Opening message
type: opening
reply: {greeting}! I'm the Archive Assistant for {title}. Ask me anything about the book, such as what it covers, who wrote it, or which chapters you can read free. How can I help?

## Reply when nothing matches
type: fallback
reply: That's a good question, but I don't have it in my notes on this book. I can search the web for you, or you might find the answer in the full edition on Amazon.
reply: I couldn't find that in the book material I have. Want me to search the web for it, or would you like to check the full book on Amazon?

## What this book is about
Cybersecurity in Healthcare, subtitled Protecting Healthcare Data in the Age of AI, is an essential guide to safeguarding medical technology and patient information. It covers real-world ransomware and data-breach case studies, advanced hacking techniques, IoT and medical-device vulnerabilities, and practical offensive and defensive strategies to protect healthcare systems. Its motto is Secure. Defend. Protect.

## Book structure and parts
The book has 13 chapters across five parts, plus a Foreword, Introduction and Conclusion: Part I, The Evolution of Human Civilization; Part II, The Anatomy of Threats and Vulnerabilities; Part III, Offensive and Defensive Security Strategies; Part IV, Advanced Topics in Healthcare Security; and Part V, Real-World Lessons and the Path Forward.

## About the author
Who wrote this book? Cybersecurity in Healthcare was written by its author, Dr. Nambili Samuel, a certified medical physician, ethical hacker (CEH), AI researcher, data scientist and cybersecurity expert with a military background in cyberwarfare. He holds MD and MBA credentials and is currently based in Mersin, Turkey. He is originally from Namibia, where he served 19 years in the Namibian Defence Force.

## Publisher and ISBN
The book is published by Tech Health Press, ISBN 978-1-23456-7890-0. The first edition was published via Amazon and Tech Health Press.

## Price, cost and where to buy
action: amazon
The book costs $20.00 and is available on Amazon. The QR code on this page also opens the Amazon listing.

## Page count, length and how many pages
The book runs 93 pages across five parts and 13 chapters.

## Reading a sample / the Foreword
action: sample
You can read the full Foreword for free, right in this preview site. Just ask to read it, or open it from the sidebar.

## Which chapters are free to read here
This preview site unlocks the Foreword plus Chapters 2, 3 and 4, each with up to four readable pages. Everything else is available in the full edition on Amazon.

## Format and editions
The book is available as a paperback on Amazon.

## Edition and publication year
This is the First edition, published in 2026.

## Background of the research
The knowledge in the book is drawn from the author's research and from the Cyber Security Industry 4.0 training program, designed for high-level technicians and engineers in industry. The program was co-funded by the Erasmus+ Programme of the European Union and developed with IDEC, the Polytechnic of Porto in Portugal, Politeknika Ikastegia Txorierri in Spain and the Hellenic Mediterranean University in Greece.

## Acknowledgments
The author thanks DS Tech Namibia, the Namibia International Cybersecurity Conference (NICSC), colleagues at Abertay University in Scotland, Science Techniz and IBM, along with several individual mentors and collaborators and his family.

## Foreword, in brief
The Foreword argues that a cyberattack on a hospital is different from one on a bank, because it endangers patient safety and human life. It reviews the digitization of healthcare, major attacks including WannaCry, Düsseldorf, the Irish HSE and a 2024 Namibian incident, and calls AI both a shield and a sword. It concludes that the question is no longer whether healthcare will be attacked, but whether it is prepared.

## Who should read it
The book is written for healthcare professionals, hospital IT administrators, cybersecurity teams and policymakers.

## Introduction: The Architecture of Modern Healthcare Technology
The Introduction explains how Electronic Health Records sit at the centre of an ecosystem of clinical systems (such as PACS and laboratory systems), administrative platforms and connected medical devices. It argues that cybersecurity is a clinical safety imperative, not just a regulatory requirement, and looks at wearables for diabetes and heart-rate monitoring and the security concerns they raise. This section is in the full edition.

## Chapter 1: The Foundation of Technology
Traces human civilization from the Stone Age through the Agricultural, Industrial and Information Ages to the AI era, then maps the architecture of modern healthcare technology, the ecosystem of specialized clinical systems such as PACS and DICOM, and why cybersecurity is a clinical imperative. This chapter is in the full edition.

## Chapter 2: Cybersecurity in a Clinical Context
action: chapter:c2
Explains the CIA Triad of confidentiality, integrity and availability in clinical terms: why patient records are so valuable, how a corrupted allergy record can be fatal, and why EHR downtime is so dangerous. Includes a technical note on the 18 HIPAA identifiers. Free to read here, across four pages.

## Chapter 3: Who Hack Healthcare and Why
action: chapter:c3
Covers ransomware-as-a-service, white, black and gray hat hackers, nation-state actors, insider threats and the reasons healthcare is a preferred target. Free to read here, across four pages.

## Chapter 4: Cyber Threats Targeting Healthcare Systems
action: chapter:c4
Opens Part II with the anatomy of a modern ransomware attack, plus phishing, supply-chain attacks and vulnerable connected devices. The opening is free to read here, across two pages.

## Chapter 5: Security Risks in Medical Devices and IoTs
Examines security risks in medical devices and the Internet of Things. This chapter is in the full edition.

## Chapter 6: Patient Data Privacy and Protection
Covers patient data privacy and protection and the regulatory frameworks around medical data. This chapter is in the full edition.

## Chapter 7: Offensive Security in Healthcare
Looks at healthcare security from the attacker's side: the offensive techniques and tools used by ethical hackers. This chapter is in the full edition.

## Chapter 8: Defensive Cybersecurity Strategies
Covers practical defensive strategies and tools to protect healthcare systems. This chapter is in the full edition.

## Chapter 9: Artificial Intelligence as a Shield
Explores how AI strengthens healthcare cyber defense, such as threat detection and incident response. This chapter is in the full edition.

## Chapter 10: Artificial Intelligence as a Sword
Explores how attackers use AI to build more sophisticated attacks. This chapter is in the full edition.

## Chapter 11: Securing Modern Healthcare Infrastructure
Covers securing modern healthcare infrastructure, including cloud platforms and connected devices. This chapter is in the full edition.

## Chapter 12: Lessons from the Case Studies
Draws practical lessons from real-world healthcare cyberattack case studies. This chapter is in the full edition.

## Chapter 13: The Future of Cybersecurity in Healthcare
Looks ahead at where healthcare cybersecurity is heading. This chapter is in the full edition.

## Conclusion
Closes the book with its key lessons and a call to act before the next major breach rather than after it. This section is in the full edition.

## Full chapter list
Here's the full chapter list: Foreword and Introduction, then 1. The Foundation of Technology, 2. Cybersecurity in a Clinical Context, 3. Who Hack Healthcare and Why, 4. Cyber Threats Targeting Healthcare Systems, 5. Security Risks in Medical Devices and IoTs, 6. Patient Data Privacy and Protection, 7. Offensive Security in Healthcare, 8. Defensive Cybersecurity Strategies, 9. Artificial Intelligence as a Shield, 10. Artificial Intelligence as a Sword, 11. Securing Modern Healthcare Infrastructure, 12. Lessons from the Case Studies, 13. The Future of Cybersecurity in Healthcare, and the Conclusion.

## Copyright and disclaimer
Copyright 2026, all rights reserved. The book is for educational and informational purposes, the author makes no warranty about completeness or accuracy given how fast this field moves, and nothing in it constitutes medical, legal, engineering or cybersecurity advice.
