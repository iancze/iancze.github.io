---
title: Syllabus
date: 2022-06-21
publishdate: 2022-06-21
toc: true
weight: 1
---

## Objective: radio astronomy and interferometric imaging

Interferometric astrophysical observations are a vast and deep topic. The goal of this course is to help students develop a **practical understanding** of how interferometers (in particular arrays like the VLA or ALMA) **observe an astrophysical source**, build a **mathematical foundation** for working with **complex-valued, Fourier-plane data**, and survey some of the many approaches that are used to **investigate astrophysical phenomena**, focusing on **forward-modeling** and **regularized maximum likelihood** techniques.

## Instructor

* Professor (Dr.) Ian Czekala (he/him/his)
* *Email*: ipc5094@psu.edu or iczekala@psu.edu (alias).

Office hours by appointment.

**If you are in any way feeling ill or suspect you might have been contact with an individual infected with COVID, *please* stay home and seek medical care if necessary.** We plan on posting all lecture notes, and we will work with you to provide you with the course materials you need.

## Course Calendar and Closure Policies

ASTRO 589 meets **once a week on Wednesdays from 10:10am to 11:00am ET (prompt) in Davey Lab Room 538**.

For full information on lecture dates and topics, see the [Course Schedule]({{<relref "schedule" >}}).

If campus should be closed (e.g. for a weather-related event or COVID precautions), the instructor will provide instructions via email on course lecture format (possibly remote, keeping the same schedule) and examinations/assignments (due dates to be rescheduled *no earlier than 48 hours* after closure announcement).

## Assignments and Project

### Course Grade

The course grade will be based on

* problem sets (55%)
* group project and presentation (45%)

### Problem sets

Problem sets will be assigned approximately every 2-3 weeks.

Please turn in homeworks in `.PDF` format to Ian by email at ipc5094@psu.edu with the subject line `ASTRO 589 problem set`. Handwritten solutions are fine, but please digitize your submission using a department scanner or a smartphone scanner app (e.g., [Adobe Scan](https://acrobat.adobe.com/us/en/acrobat/mobile/scanner-app.html)).

Due dates will be set to correspond to the beginning of a class period (i.e., 10:10am).

#### Programming

For problem sets, students are encouraged to use whatever programming language they are most comfortable with.

Some project choices, especially those involving CASA, may require basic familiarity with the Python programming language.

#### Problem set late policy

The following percentages will be deducted from your score

* one day late: 5%
* two days late: 10%
* three days late: 25%
* more than 72 hours late: no credit

If extenuating circumstances arise such that you will be unable to complete the homework on time, please contact Ian *before* the homework deadline and we can most likely arrange an extension.

#### Problem set collaboration policy

You are encouraged to collaborate and work through the problem sets together. However, each student must complete the final write up on their own, i.e., no problem set should be duplicated verbatim between students.

### Group project and presentation

Students will form groups of 2-3 for their course projects. Groups will propose a project concept around some aspect of interferometry and discuss in detail at least one major astrophysical application. The project idea may be based of the list of example projects (below) or may be an original idea proposed by the group.

The project will culminate in a presentation to the class. This presentation will be a comprehensive lecture on your topic, which should take 35 minutes of our class period, followed by 15 minutes of detailed questions and answers. Students may submit any additional materials beyond their presentation for review, if so desired; however, no final report is required.

Good presentations will:

* Provide a cogent astrophysical and technical introduction
* Connect to course material covered (if applicable)
* Introduce the key supporting background material, including any relevant equations or seminal figures from related works
* Copious citations to and explanations of relevant literature
* Explain the key observational or theoretical instruments/methodologies (as applicable)
* Identify at least one major astrophysical application and discuss in detail
* Contain some new aspect of technical development or investigation by the group
* Finish the presentation within the allotted time (35 +/- 5 minutes)
* Adequately answer questions raised by the instructor and students

Each group should provide the instructor with the two dates that they prefer by **Wednesday, September 21st**. The instructor will then assign one presentation date to each group. You should notify the instructor of your project topic at least two weeks prior to your presentation date to confirm that it is an acceptable choice.

It is a wise idea to practice your presentation from start to finish "live" to make sure your timing is correct. Groups whose talks are wildly under/over time will find it difficult to achieve full credit.

#### Example Project Ideas

* Cross-validation strategies for imaging (including for RML)
* How do MRI imagers work, and what are their fundamental relationships to radio interferometers?
* Optical interferometers and science results
* Download a dataset from the ALMA archive, image with CLEAN or RML techniques
* Calibration techniques for ALMA and EHT
* Self-calibration theory and application with CASA
* Spectral line capabilities of ALMA, image a molecular line from ALMA archive
* Wavelets: what are they, how have they been used in radio astronomy applications
* Design new ALMA observations using CASA/simobserve
* Expanding MPoL to do [model fitting with Pyro](https://github.com/MPoL-dev/MPoL/issues/33)
* Expanding MPoL for [fake data generation](https://github.com/MPoL-dev/MPoL/issues/77)
* Expanding MPoL to use [nufft for gridding](https://github.com/MPoL-dev/MPoL/issues/17)
* Expanding MPoL to do [self-calibration](https://github.com/MPoL-dev/MPoL/issues/24)
* Expanding MPoL to do [primary beam corrections](https://github.com/MPoL-dev/MPoL/issues/23)
* Wide-field imaging: what changes about the imaging assumptions, applications
* Beam-forming and applications

## Reference Materials

There are many additional resources that will be helpful during this course (and beyond) and will be called out in the course at the appropriate juncture. Many of these resources are freely available online or through the [University library](https://libraries.psu.edu/).

### Textbooks

* [Essential Radio Astronomy](https://www.cv.nrao.edu/~sransom/web/xxx.html) by James Condon and Scott Ransom
* [Tools of Radio Astronomy](https://catalog.libraries.psu.edu/catalog/2625688) by Rohlfs and Wilson
* [Interferometry and Synthesis in Radio Astronomy](https://catalog.libraries.psu.edu/catalog/20789467) by Thompson, Moran, and Swenson
* [Fourier Analysis and Imaging](https://catalog.libraries.psu.edu/catalog/34517505) by R. Bracewell
* [The Fourier Transform and its Applications](https://catalog.libraries.psu.edu/catalog/2010095) by R. Bracewell

### Courses

* 18 NRAO Synthesis Imaging School [slides and lectures](https://web.cvent.com/event/b7f82cf3-7126-4a71-a88b-7a93c66a4dc7/websitePage:6bbf1462-9f9f-4204-bd93-36032f793bc4)

### Videos

* [Cells to Galaxies Speaker Series Archive](https://web.cvent.com/event/1ecded89-273f-4f8a-80e8-f5eb2d40d8c5/websitePage:4967d7b9-05fe-4b32-820d-1f1587665ee3), in particular talks by [Urvashi Rao](https://vimeo.com/639606652/a927105526) and [Sanjay Bhatnagar](https://vimeo.com/612017530/9fc70df0ef) (opening ~15 minutes)
* Lectures by David Wilner [Part I](https://www.youtube.com/watch?v=0TwnZhiEc3A&ab_channel=AnitaChapter) and [Part II](https://www.youtube.com/watch?v=mRUZ9eckHZg&ab_channel=AnitaChapter)

## Masking Policy

We encourage you to get COVID-19 vaccinated and follow University policies on masking, especially in indoor spaces. In consultation with background rates in Centre County, masks may be required part or all of the fall semester. Please consult [PSU VirusInfo](https://virusinfo.psu.edu/) for the latest policy.

## Academic Integrity

Academic integrity is the pursuit of scholarly activity in an open, honest and responsible manner. Academic integrity is a basic guiding principle for all academic activity at The Pennsylvania State University, and all members of the University community are expected to act in accordance with this principle. Consistent with this expectation, the University’s Code of Conduct states that all students should act with personal integrity, respect other students’ dignity, rights and property, and help create and maintain an environment in which all can succeed through the fruits of their efforts.

Academic integrity includes a commitment by all members of the University community not to engage in or tolerate acts of falsification, misrepresentation or deception. Such acts of dishonesty violate the fundamental ethical principles of the University community and compromise the worth of work completed by others.

## Disability Accommodation

Penn State welcomes students with disabilities into the University’s educational programs. Student Disability Resources (SDR) website provides contact information for every Penn State campus (Links to an external site.). For further information, please visit [Student Disability Resources website](http://equity.psu.edu/student-disability-resources/).

In order to receive consideration for reasonable accommodations, you must contact the appropriate disability services office at the campus where you are officially enrolled, participate in an intake interview, and provide documentation: See [documentation guidelines](http://equity.psu.edu/student-disability-resources/applying-for-services/documentation-guidelines). If the documentation supports your request for reasonable accommodations, your campus disability services office will provide you with an accommodation letter. Please share this letter with your instructors and discuss the accommodations with them as early as possible. You must follow this process for every semester that you request accommodations.

## Counseling and Psychological Services

Many students at Penn State face personal challenges or have psychological needs that may interfere with their academic progress, social development, or emotional wellbeing. The university offers a variety of confidential services to help you through difficult times, including individual and group counseling, crisis intervention, consultations, online chats, and mental health screenings. These services are provided by staff who welcome all students and embrace a philosophy respectful of clients’ cultural and religious backgrounds, and sensitive to differences in race, ability, gender identity and sexual orientation.

* Counseling and Psychological Services at University Park  [CAPS](http://studentaffairs.psu.edu/counseling/): 814-863-0395
* [Counseling and Psychological Services at Commonwealth Campuses](https://senate.psu.edu/faculty/counseling-services-at-commonwealth-campuses/)
* Penn State Crisis Line (24 hours/7 days/week): 877-229-6400
* Crisis Text Line (24 hours/7 days/week): Text LIONS to 741741

## Educational Equity and Reporting Bias

Penn State takes great pride to foster a diverse and inclusive environment for students, faculty, and staff. Acts of intolerance, discrimination, or harassment due to age, ancestry, color, disability, gender, gender identity, national origin, race, religious belief, sexual orientation, or veteran status are not tolerated and can be reported through Educational Equity via the [Report Bias webpage](http://equity.psu.edu/reportbias/).

*Whom should I contact if I need additional assistance?*
I encourage you to be in contact with your academic adviser for specific needs you might have outside this course.  Academic adviser information and scheduling can be found at <https://sites.psu.edu/starfishinfo/>. There are also additional resources available at <https://keeplearning.psu.edu/student-support/>

### Counseling & Psychological Services (CAPS)

* <https://studentaffairs.psu.edu/caps-contact-form>
* CAPS Phone: (814) 863-0395
* Penn State Crisis Line 1-877-229-6400
* Student Care and Advocacy: Email: StudentCare@psu.edu
* Share a Concern: [Share a Concern Form](https://cm.maxient.com/reportingform.php?PennState&layout_id=14) Phone: 814-863-2020 (voicemail)

## Code of Mutual Respect and Cooperation

The Eberly College of Science (ECoS) [Code of Mutual Respect and Cooperation](https://science.psu.edu/climate-and-diversity/code-mutual-respect-and-cooperation) embodies the values that we hope
faculty, staff, and students possess and will endorse to make ECoS a place where every individual feels respected and valued, as well as challenged and rewarded. Please review these principles, linked [here](https://science.psu.edu/climate-and-diversity/code-mutual-respect-and-cooperation).
