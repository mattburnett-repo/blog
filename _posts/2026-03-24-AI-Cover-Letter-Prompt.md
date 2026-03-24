---
title: AI Prompt to Generate Cover Letters
---

### What
Although it's already been "done to death", here's an AI prompt to generate a cover letter. I wrote it to speed up the job application process, not replace my participation in it. 

Which is a round-about way of saying that I already know how to write a cover letter, and just want to speed the process along, rather than just ask the AI to do the the whole thing for me, without my involvement. It just automates tasks I've been doing, allowing me to 
get the tasks done more quickly. Velocity, and all that...

The use case for this prompt is that you've already read the job description and want to apply, and that a cover letter is part of the process, not that "you just want to spam a bunch of recruiters to see what happens".

### How
The prompt asks if you need a cover letter for
- A specific job application.
- A 'general consideration' application.

If it's for a specific application, the prompt asks for the job description. This is a simply copy/paste of the job posting, which I assume you've already read.

In either case, it asks for the text from your resume. Another simple copy/paste.

You can easily revise the output in-prompt.

When you're satified with the output, you can copy/paste into what format you need for the cover letter.

It does not automate the entire job application process. An automated job application process could easily turn into a spam machine, and that's not the intent of this prompt. Please don't use it that way.

I've run this in ChatGPT and Cursor/Claude. In both cases the output is reasonable, but as already mentioned, can be modified in-prompt.

### TL;DR
Here's the prompt. It is tailored for the DC job market, but you could easily adapt it to your locale:

```
# ChatGPT Cover Letter Generator – Auto-Execute Prompt (Updated)

Use only the information provided below. Do not assume or add anything else.

## Instructions
1. Ask the user whether this is:  
   - (A) A specific job application, or  
   - (B) A general consideration application  

2. If the user selects **(A) Specific Job Application**:  
   - Ask the user to paste the full Job Description (including Job Summary, Minimum Requirements, Knowledge/Skills/Abilities, Essential Duties, Additional Duties).  
   - Ask the user to paste the full Resume (including Professional Summary, Technical Skills, Professional Experience, Education/Certifications).  
   - After receiving both, generate a concise, professional, ready-to-send cover letter using only the information explicitly provided.  
   - Include a professional salutation (e.g., "Dear Hiring Team,") at the beginning.  

3. If the user selects **(B) General Consideration**:  
   - Ask the user to paste the full Resume (including Professional Summary, Technical Skills, Professional Experience, Education/Certifications).  
   - After receiving the resume, generate a concise, professional, ready-to-send cover letter using only the information explicitly provided.  
   - Include a professional salutation (e.g., "Dear Hiring Team,") at the beginning.  
   - Offer three variants automatically:  
     1. **Standard version** (~170 words)  
     2. **Punchy recruiter-optimized version** (~150 words)  
     3. **Ultra-lean scan-friendly version** (~100–130 words)  

## Cover Letter Requirements

### Tone & Style
- Professional, confident, personable; avoid AI-sounding phrasing, generic buzzwords, or filler.  
- Ensure the text reads like a human wrote it; clear, natural sentences.  

### Output Format
- **Do not include a header** (name, contact info, links, or date) at the top.  
- Keep signature section only at the end in this format:

Best regards,  
Matt Burnett  
Portfolio: https://mattburnett-repo.github.io/portfolio-website/  
GitHub: https://github.com/mattburnett-repo  
Blog: https://github.com/mattburnett-repo/blog

### Content Flow

#### Specific Job Application
1. **Salutation:** Begin with a professional greeting (e.g., "Dear Hiring Team,").  
2. **Introduction:** Express interest in the role; summarize experience or projects explicitly relevant to the job.  
3. **Highlight:** Focus on relevant skills, technologies, or achievements that match the job description.  
4. **Flexibility & Fit:** Keep brief and forward-looking.  
5. **Closing:** Include availability and polite closing; offer to discuss experience or provide additional information.  

#### General Consideration (Three-Variant Structure)
- **Paragraph 1: Introduction**  
  - Express interest in the organization or potential opportunities.  
  - Position yourself in 1–2 sentences, highlighting your professional identity and core strengths.  
  - Avoid listing all skills; frame yourself as a strong candidate with immediate potential impact.  

- **Paragraph 2: Highlight / Fit**  
  - Present 2–3 of your strongest, most relevant skills, experiences, or achievements.  
  - Emphasize contributions that signal immediate value (e.g., cross-functional work, system modernization, mentoring).  
  - Keep sentences clear, concise, and signal-oriented. Avoid dense skill lists or repeating the resume.  
  - End with a forward-looking statement showing flexibility and willingness to discuss opportunities.  

- **Variant Requirements:**  
  1. **Standard (~170 words):** Balanced, full narrative with strong signal.  
  2. **Punchy (~150 words):** Optimized for recruiter scanning, attention-grabbing first sentence.  
  3. **Ultra-Lean (~100–130 words):** Blink-and-it-reads version, strongest signals only, maximal readability.  

### Length & Focus Constraints
- **Specific job:** Standard concise cover letter length (3–4 short paragraphs).  
- **General consideration:** See variant requirements above.  

### Behavior
- Only reference facts explicitly in the provided resume or job description.  
- Do not use placeholders, brackets, or generic filler.  
- Immediately generate the cover letter once all required inputs are received.  
- Ensure readability for quick scanning by recruiters; every sentence should convey strong signal.  
- Include a professional salutation at the beginning for all outputs.
```
