import fitz  # PyMuPDF

# Open the original PDF
original_pdf = '/mnt/agents/upload/Letter_of_Appreciation_Janhavi_Deshpande.pdf'
output_pdf = '/mnt/agents/output/Letter_of_Invitation_Janhavi_Deshpande.pdf'

doc = fitz.open(original_pdf)
page = doc[0]

# Get page dimensions
page_rect = page.rect

# Create overlay for new text
overlay = fitz.open()
overlay_page = overlay.new_page(width=page_rect.width, height=page_rect.height)

# Text positions and content based on original layout
# Title position (was "LETTER OF APPRECIATION")
title_rect = fitz.Rect(197, 125, 419, 143)
overlay_page.insert_text(
    title_rect.tl,
    "LETTER OF INVITATION",
    fontsize=13,
    fontname="helv",
    color=(0, 0, 0)
)

# Date position (was "Date: 28 September 2026")
date_rect = fitz.Rect(72, 463, 195, 480)
overlay_page.insert_text(
    date_rect.tl,
    "Date: 21 September 2026",
    fontsize=11,
    fontname="helv",
    color=(0, 0, 0)
)

# Recipient section (was "Presented to", "Ms. Janhavi Deshpande", "Ethical Hacker...")
# Replace with To, Ms. Janhavi Deshpande, Ethical Hacker & Cyber Forensic Investigator
overlay_page.insert_text(
    fitz.Rect(72, 156, 200, 173).tl,
    "To,",
    fontsize=11,
    fontname="helv",
    color=(0, 0, 0)
)

overlay_page.insert_text(
    fitz.Rect(72, 184, 250, 201).tl,
    "Ms. Janhavi Deshpande",
    fontsize=11,
    fontname="helv",
    color=(0, 0, 0)
)

overlay_page.insert_text(
    fitz.Rect(72, 212, 350, 229).tl,
    "Ethical Hacker & Cyber Forensic Investigator",
    fontsize=11,
    fontname="helv",
    color=(0, 0, 0)
)

# Body text - paragraph 1 (was "We sincerely appreciate...")
body1_rect = fitz.Rect(72, 239, 535, 285)
body1_text = """Subject: Invitation as Guest Speaker for an Expert Session

Dear Ma'am,

We are pleased to invite you as a Guest Speaker for an expert session organized for our students on the topic:

"Ethical Hacking & Cyber Forensics: Defending Modern Digital Infrastructure"

Your extensive experience as an Ethical Hacker and Cyber Forensic Investigator, including your Master's degree in Information and Cybersecurity, your collaboration with premier Indian government agencies such as the Income Tax Department, SEBI, GST, and the Enforcement Directorate (ED), and your expertise in digital forensics, data recovery, malware analysis, incident response, penetration testing, vulnerability assessment, and cybersecurity training, makes your insights highly valuable to our students."""

# Use textbox for better control
overlay_page.insert_textbox(
    body1_rect,
    body1_text,
    fontsize=10,
    fontname="helv",
    color=(0, 0, 0),
    align=0  # Left align
)

# Body text - paragraph 2 (was "Your valuable insights...")
body2_rect = fitz.Rect(72, 295, 555, 355)
body2_text = """The session aims to provide students with practical insights into ethical hacking, cyber forensics, incident response, malware analysis, and proactive cyber hygiene, and to help them understand how security professionals defend modern digital environments.

We would be honored by your presence and look forward to an insightful and engaging interaction with our students.

Thank you for considering our invitation. We sincerely look forward to welcoming you."""

overlay_page.insert_textbox(
    body2_rect,
    body2_text,
    fontsize=10,
    fontname="helv",
    color=(0, 0, 0),
    align=0
)

# Body text - paragraph 3 (was "Your expertise...")
body3_rect = fitz.Rect(72, 365, 555, 415)
body3_text = """Warm regards,"""

overlay_page.insert_textbox(
    body3_rect,
    body3_text,
    fontsize=10,
    fontname="helv",
    color=(0, 0, 0),
    align=0
)

# Signatures are already in the original PDF at the bottom
# They will remain as is: Prof. Leena Patil [Faculty Co-ordinator] and Dr. Tabassum Maktum [Faculty Advisor]

# Merge overlay with original page
page.show_pdf_page(page_rect, overlay, 0)

# Save the result
doc.save(output_pdf)
doc.close()

print(f"PDF created successfully at: {output_pdf}")
