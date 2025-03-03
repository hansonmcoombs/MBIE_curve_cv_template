Joint CURVE and MBIE RS&T CV Template
##########################################

The goal of this repository is to provide a template for generating both a nice looking CURVE based CV and a MBIE RS&T CV from the same underlying data source files, so that a researcher only needs to maintain one set of data files.

This template is based on the Curve CV template by LianTze Lim (
https://www.overleaf.com/latex/templates/curriculum-vitae-of-liantze-lim/qzvdbpzvzjxw) and the MBIE RS&T CV template (https://www.mbie.govt.nz/dmsdocument/25174-curriculum-vitae-cv-templates-when-applying-for-funding).

Latex Version
================
The template was constructed using the TexLive distribution of Latex (sudo apt-get install texlive-full). The template was tested on Xubuntu 24.04

Outstanding issues and inconsistencies with the MBIE RS&T CV
=================================================================

* The pythontex had some issues with the path to the python interpreter.  This was resolved by a minor modification to the pythontex package.  This modification is documented below.
* MBIE RS&T CV formatting discrepancies:
    * The personal details section has slightly different sub-column widths as compared to the word template, this was done to simplify the latex implementation of the table.
    * Sections 1b., 1c., 1d., 1f. are contained within tcolorboxes (boxes) as opposed to the word template where they are blank lines (no frame surrounding the text).
    * The font **helvet** is used in place of the proprietary **Arial** font to support linux systems.
    * Section 1g. "patents" has been replaced with "consultancy reports"
    * Section 2a. the subtitles (Peer-review journal articles) are bolded rather than plan text (as in the word template).  This was done for readability only
    * Section 2a. The user's orcid id (live link) has been added to the "Peer-review journal articles" subtitle.
    * Section 2a. The references are not within in a tcolorbox (frame) as in the word template.  This is due to a bug which prevents the \printbibliography command from working within a tcolorbox if a filter or heading=none is used.
    * Section 2c. and 2d. have been incorporated into tcolorboxes (frames) for consistency with the rest of the CV.
* Note that the mbie_settings.sty file was taken from the Curve CV template by LianTze Lim and largely used to implement the bibliography formatting (bolding author names). I have removed as much excess as I could, but not all of the settings may be needed.


CURVE CV
================

this uses the data from the ./data folder to generate a CURVE cv.

Specifically it calls data from:

* data/career_summary.tex: define the career summary text in the CV
* data/details.tex: define the personal details in the CV, see comments for details
* data/distinctions_memberships.tex: define the distinctions and memberships in the CV
* data/education.tex: define the education in the CV
* data/employment.tex: define the employment in the CV
* data/open_source_projects.tex: define the open source projects in the CV
* data/own-bib.bib: define the bibliography in the CV
* data/photo.png: photo for the CV, note the path can be changed in data/details.tex
* data/project_examples.tex: define the project examples in the CV
* data/research_speciality.tex: define the research speciality in the CV
* data/skills.tex: define the skills in the CV

MBIE RS&T CV
================

The MBIE RS&T CV is generated from the same data files as the CURVE CV, but with a different set of latex files.

* data/dem_end_user_relationships.tex: the demonstrate end user relationships in the MBIE RS&T CV
* data/details.tex: define the personal details in the CV, see comments for details
* data/distinctions_memberships.tex: define the distinctions and memberships in the CV
* data/education.tex: define the education in the CV
* data/employment.tex: define the employment in the CV
* data/impact_previous_work.tex: define the impact of previous work in the MBIE RS&T CV
* data/mbie_prev_research.tex: define the previous research in the MBIE RS&T CV
* data/own-bib.bib: define the bibliography in the CV
* data/research_speciality.tex: define the research speciality in the CV


Pythontex modification
=======================

The pythontex package has been modified to pass the sys.executable path to the python interpreter for the popen call.  This must be done on the system wide package:

Adjustment to c. line 1575 /usr/share/texlive/texmf-dist/scripts/pythontex/pythontex3.py (run_code)

A github issue has been opened to address this issue: https://github.com/gpoore/pythontex/issues/222

.. code-block:: python
      # new line:
      exec_cmd[0] =  f'{sys.executable}'   # this switches the interpreter from "python" to the path to the python interpreter which is passed in the sys.executable variable
       # unmodified lines:
       try:
           if family != 'Rcon':
               proc = subprocess.Popen(exec_cmd, stdout=out_file, stderr=err_file)
               print('run')



Special bold font
====================

There are two special bold font commands that can be used in the CV and should be used in place of \textbf{}.

* \mbiebf{} - This is for bolding text that will appear bold in both the Curve CV and the MBIE RS&T CV.
* \cvtextbf{} - This is for bolding text that will appear bold in the Curve CV but not in the MBIE RS&T CV.

note: the \cvtextbf{} command is set to no action in the MBIE RS&T CV.

Extra details
====================

The MBIE RS&T CV has a very bare bones approach for details on the education and employment sections.
To support adding more detail to the CURVE CV, a toggle has been set (detailedversion) which is set to true in the CURVE CV and false in the MBIE RS&T CV.

To add more detail to a section:

.. code-block:: latex

    \entry[2000 -- 2001] {\cvtextbf{Agent of entropy}, The universe
    \iftoggle{detailedversion}{details that will only be printed in the CURVE cv}{~}}

Transition between ruibric and enumerate
=========================================

* To support both CURVE and MBIE RS&T CVs, the \entry command has been redefined in the MBIE RS&T CV to produce a \item command for the enumerate environment. This means that entries must be specified as "\entry[dates]{text}" not \entry*[dates] text.

* the rubric environment has been redefined in the MBIE RS&T CV to produce a \begin{enumerate} environment.

Projects and Open Source Projects approach
===============================================

The projects and open source projects and to a lesser extent the employment sections are expected to have a full list of one's projects and employment.
The filtering approach for a given use of a CV is to simply comment out the \entry commands that are not relevant to the CV being generated.


Bibliography Approach
========================

The bibliography is generated using the biber compiler and the biblatex package with formatting (bolding author names) defined in the mbie_settings.sty and CURVE_settings.sty files courtesy of LianTze Lim.

The bibliography is generated from the data/own-bib.bib file and is included in the CV using the \printbibliography command.
The bibliography filters are defined in the top level files (CURVE_cv.tex and MBIE_cv.tex) and are used to filter the bibliography entries based on the bibtype and keywords defined in the data files.

Presently the bibliography is filtered based on the bibtype and keywords.
Only references including the keyword "show" will be included in the bibliography.
This allows easy changes to the bibliography without having to change the main CV files. (only the data/own-bib.bib file needs to be updated)

If you are using Zotero to manage your personal bibliography you can set the keywords by assigning tags (e.g. the "show" tag) to the references you want to include in the CV before exporting the bibliography to a .bib file.

Note that Zotero will not export the year={in submission} field to the .bib file, so this must be added manually to the .bib file if you want to include references that are in submission.

The section 1g. counts of publications in the MBIE RS&T CV is generated by bespoke python code that counts the nubmer of publications that have the appropriate type (e.g. "@article{") in the data/own-bib.bib file.
All references are counted even those without the "show" keyword.
This python code is defined in mbie_rsnt_settings/pythonsetup.tex and requires only included python packages (datetime, re, pathlib) to run.


Note there is a toggle in the CURVE_cv.tex file (\settoggle{allrefs}{true}) that when set to true will include all references in the bibliography that do not have the keyword "private" in the data/own-bib.bib file.  This is useful for generating a full bibliography without disclosing potentially private references.

Note that special characters in the .bib file (e.g., subscripts and other unicode characters) can cause the bibliography to fail to compile.  These characters should be removed from the .bib file.


(Bibtex configuration)
------------------------

**Compiler**

Biber

**Environemnt Variables**

BIBINPUTS=~/PycharmProjects/cv/example_cv;BSTINPUTS=~/PycharmProjects/cv/example_cv:

**main file that includes the bibliography**
~/PycharmProjects/cv/example_cv/cv-llt.tex

**working directory for bibtex**

~/PycharmProjects/cv/out


