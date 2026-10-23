# 3. Literature Review


## 3.1 Method

Sources were collected in October 2026 through web search of scholarly venues (arXiv, ACM Digital Library, USENIX, PeerJ, Frontiers, CAIS, ScitePress) and the websites of the tools discussed. Search topics:

1. proctoring and cheating detection in online programming exams;
2. academic integrity in programming contests and courses;
3. student and faculty perceptions of remote proctoring (privacy, anxiety, false accusations);
4. plagiarism detection for source code, including LLM-generated code;
5. DOMjudge and automated judging in teaching and contests;
6. browser-based integrity signals (tab switching, copy/paste).

Selection criteria: relevance to online programming assessment or contests; preference for sources from 2021 to 2026; foundational or industrial sources only where needed to describe existing tools. Result: 17 sources, of which 10 are peer-reviewed publications, 5 are preprints and 2 are tool documentation or industry sources.

## 3.2 Review

### 3.2.1 Proctoring of online programming exams

Heavy webcam- and vision-based proctoring dominates the literature. A framework for multimodal proctoring [13] combines computer vision for faces, objects and hand-object interaction, and states openly that human proctoring does not scale while many automated solutions are intrusive. Work on abnormal behavior detection in online exams [12] follows the same direction.

For programming exams specifically, Ortin et al. [1] propose a machine-learning assistant that analyzes sequences of screenshots with a CNN and RNN to help instructors detect fraud in real time. Their paper also recalls earlier systems with very high accuracy and recall but extremely low precision, which illustrates that false positives are the central difficulty of automated detection.

Studies of behavior-based signals are fewer. Akçapınar [11] analyzed exam logs of 52 students with clustering, using counts of text selections, right-clicks and page focus losses as indicators of possible AI-assisted cheating. The study is preliminary, but it shows that cheap browser events carry information about suspicious behavior without cameras.

### 3.2.2 Cheating and integrity in programming

Goldberg [6], writing from the experience of teaching online programming courses, argues that standard anti-cheating measures are insufficient and that cheating is more widespread than many assume, including through paid help. He recommends redesigning course assessment rather than relying on a single tool.

Industry practice documents the same logic. HackerRank [17] describes tab proctoring that detects tab and window focus changes and copy-paste activity, and also lists what such tools miss: other devices, mobile hotspots and browser extensions that hide switching. This supports the position that browser events are useful as evidence, but not as proof.

A study of plagiarism in competitive programming [14] notes that tasks with unique known solutions make copying hard to distinguish, and advocates combining detection algorithms, mastery-based learning and authentic assessment.

### 3.2.3 Plagiarism detection and generative AI

Similarity detectors compare submitted code. Gacek et al. [9] evaluate MOSS, JPlag, DOLOS and copydetect and report a trade-off between sensitivity and specificity: some tools resist false positives but are vulnerable to obfuscation, others the opposite.

Generative models make this harder. Biderman and Raff [7] showed that a language model could complete introductory programming assignments without triggering MOSS, and that the generated code varied in structure with no obvious signature. Maisch et al. [8] asked students to plagiarize on purpose and document how: few succeeded in evading detection, but the strategies were varied and often LLM-based.

Taken together, post-hoc code similarity analysis alone is not enough for a contest where participants may use an LLM. Behavioral logs from the participant's side are a complementary source of evidence.

### 3.2.4 Student and faculty perceptions of proctoring

Alessio and Messinger [3] surveyed 150 faculty and 78 students: most students and about half of faculty saw proctoring as an invasion of privacy, and students reported stress and anxiety. Balash et al. [2] analyzed browser extensions of proctoring services and surveyed students: students accept some loss of privacy but are concerned about sharing personal data, and institutional power dynamics may reduce open opposition. Meulmeester et al. [4] surveyed 597 medical students; the main worries were wrongful invalidation of an exam due to connection, noise or webcam problems, and the fear of being flagged incorrectly. Bergmans et al. [5] found that students using a commercial tool considered it easy to set up and believed that proctoring itself deters cheating.

Common conclusions: false flags, technical failures and privacy are the main concerns, while deterrence is perceived as the main benefit.

### 3.2.5 DOMjudge and automated judging

DOMjudge was designed for ICPC-style contests, has been in use for over two decades, handles sandboxing of untrusted code and is language-independent by design [15]. It is also used in teaching: the Hong Kong Polytechnic University case study [10] used DOMjudge to judge a Python contest for an introductory course. DOMjudge exposes a REST API and has jury, team and admin roles [16], which makes integration possible without modifying the judging engine. The literature on DOMjudge focuses on grading and learning outcomes, not on behavioral integrity control.

## 3.3 Structured analysis table

| # | Source | Type | Year | Topic | Method / data | Main finding | Limitation for our project |
|---|---|---|---|---|---|---|---|
| 1 | Ortin et al., PeerJ CS | Journal | 2025 | ML detection in online programming exams | CNN+RNN on screenshot sequences | ML can assist instructors in real-time detection | Needs screen capture; false-positive risk; heavy |
| 2 | Balash et al., SOUPS | Conf. | 2021 | Student privacy perceptions | Review analysis of extensions, survey n=102 | Privacy concerns, trust in institution shapes acceptance | General exams, not programming |
| 3 | Alessio, Messinger, Front. Educ. | Journal | 2021 | Faculty/student perceptions | Survey, 150 faculty, 78 students | Privacy invasion and stress are widespread concerns | Pandemic context, not contests |
| 4 | Meulmeester et al. | Journal | 2021 | Student perspectives | Survey, 597 students | Fear of wrongful invalidation and false flags | Medical progress tests |
| 5 | Bergmans et al. | Conf. | 2021 | Efficacy of Proctorio | Student study | Easy to use; seen as deterrent | One commercial tool |
| 6 | Goldberg, CAIS | Journal | 2021 | Integrity in online coding courses | Experience-based analysis | Existing measures insufficient; redesign assessment | Essay, no quantitative data |
| 7 | Biderman, Raff, CIKM | Conf. | 2022 | LLM vs. plagiarism detectors | Experiment with GPT-J and MOSS | LLM code evades MOSS | Older model |
| 8 | Maisch et al. | Conf. | 2026 | How students plagiarize | Empirical challenge study | Varied manual and LLM strategies | Assignments, not live contests |
| 9 | Gacek et al., CSEDU | Conf. | 2026 | Plagiarism detectors comparison | Evaluation of 4 tools | Sensitivity/specificity trade-offs | Post-hoc analysis only |
| 10 | Yuen et al. (VERIFY authors/venue) | Paper | VERIFY | DOMjudge in a Python contest | Case study at PolyU | DOMjudge suits contest-style teaching | No integrity control |
| 11 | Akçapınar, arXiv:2510.18881 | Preprint | 2025 | AI-assisted cheating from behavior | Clustering, 52 students | Focus loss, selections, right-clicks as indicators | Preliminary, small sample |
| 12 | arXiv:2402.12179 | Preprint | 2024 | Abnormal behavior detection | Monitoring system | Describes detection approach | Not programming-specific |
| 13 | AutoOEP, arXiv:2509.10887 | Preprint | 2025 | Multimodal proctoring | Computer vision | Combines face, object, hand tracking | Intrusive, webcam required |
| 14 | arXiv:2505.08244 (VERIFY title/authors) | Preprint | 2025 | Plagiarism in competitive programming | Literature-based analysis | Multi-faceted approach needed | Review, not a tool |
| 15 | Meggendorfer, DOMtutor, arXiv:2608.16899 (VERIFY) | Preprint | 2026 | DOMjudge as autograder | Description and use cases | DOMjudge mature, versatile, sandboxed | Teaching focus |
| 16 | DOMjudge | Tool site | n/a | Judging system | Documentation | REST API, roles, judgehosts | n/a |
| 17 | HackerRank, Tab-Proctoring | Industry | VERIFY | Browser proctoring | Vendor article | What tab proctoring catches and misses | Vendor source |

## 3.4 Gaps and how the project takes them into account

| Gap in the literature | How the project responds |
|---|---|
| Most proctoring relies on webcam/CV, which students find intrusive and which raises false flags [2, 3, 4, 13] | No webcam or video. Browser-level events only (tab switch, fullscreen exit, copy/paste) |
| High recall with very low precision in automated detection [1] | The system outputs a risk score and evidence log; the teacher decides. No automatic accusation |
| Few studies use cheap behavioral signals for programming exams [11] | Event logging on the client with server-side validation, evaluated in a pilot contest |
| Browser signals can be bypassed [17] | "Prevention plus recording"; limitations documented and bypass tests planned |
| LLMs defeat code-similarity detection [7, 8, 9] | Behavioral log complements post-hoc analysis |
| DOMjudge literature focuses on grading, not integrity [10, 15] | Integrate with DOMjudge through its REST API and add proctoring on top |

## 3.5 References

[1] F. Ortin, A. Gago, J. Quiroga, M. Garcia. A machine learning assistant for detecting fraudulent activities in synchronous online programming exams. PeerJ Computer Science, 11:e3159, 2025. https://doi.org/10.7717/peerj-cs.3159

[2] D. G. Balash, D. Kim, D. Shaibekova, R. A. Fainchtein, M. Sherr, A. J. Aviv. Examining the Examiners: Students' Privacy and Security Perceptions of Online Proctoring Services. SOUPS 2021, pp. 633-652. https://www.usenix.org/conference/soups2021/presentation/balash

[3] H. M. Alessio, J. D. Messinger. Faculty and Student Perceptions of Academic Integrity in Technology-Assisted Learning and Testing. Frontiers in Education, vol. 6, 2021. https://doi.org/10.3389/feduc.2021.629220

[4] F. L. Meulmeester, E. A. Dubois, C. Krommenhoek-van Es, P. G. M. de Jong, A. M. J. Langers. Medical students' perspectives on online proctoring during remote digital progress test. 2021. https://doi.org/10.1007/s40670-021-01420-w

[5] L. Bergmans, N. Bouali, M. Luttikhuis, A. Rensink. On the Efficacy of Online Proctoring using Proctorio. 2021. https://doi.org/10.5220/0010399602790290
[6] D. M. Goldberg. Programming in a Pandemic: Attaining Academic Integrity in Online Coding Courses. Communications of the AIS, 48, pp. 47-54, 2021. https://doi.org/10.17705/1CAIS.04807

[7] S. Biderman, E. Raff. Fooling MOSS Detection with Pretrained Language Models. Proc. CIKM 2022, pp. 2933-2943. 
https://doi.org/10.48550/arXiv.2201.07406

[8] R. Maisch, L. Schmid, R. Glassey, D. Fuchss, N. Niehues, H. Liu, A. Koziolek. Criminal Minds: How First-Year CS Students Plagiarize Code. 2026. https://doi.org/10.1145/3803437.3805789

[9] P. Gacek, B. Gdowski, K. Szymanski, W. Zmuda. Comparative analysis of non-commercial plagiarism detectors for computer science education. Proc. CSEDU 2026. https://doi.org/10.5220/0014836500004021

[10] Yuen et al. Competitive programming case study with DOMjudge, Hong Kong Polytechnic University. VERIFY authors, title, year, venue. https://ira.lib.polyu.edu.hk/handle/10397/97220

[11] G. Akçapınar. Detecting AI-Assisted Cheating in Online Exams through Behavior Analytics. arXiv:2510.18881, 2025. https://arxiv.org/abs/2510.18881

[12] Examining Monitoring System: Detecting Abnormal Behavior In Online Examinations. arXiv:2402.12179, 2024. VERIFY authors. https://arxiv.org/abs/2402.12179

[13] AutoOEP: A Multi-modal Framework for Online Exam Proctoring. arXiv:2509.10887, 2025. https://arxiv.org/abs/2509.10887

[14] Plagiarism in competitive programming and online judge systems. arXiv:2505.08244, 2025. VERIFY title and authors. https://arxiv.org/abs/2505.08244

[15] T. Meggendorfer. DOMtutor: Automated Autograding for Logic in Computer Science. arXiv:2608.16899, 2026. VERIFY. https://arxiv.org/abs/2608.16899

[16] DOMjudge: Programming contest jury system. https://www.domjudge.org/ .

[17] HackerRank. Tab-Proctoring: What It Catches (and What It Misses). 2025. https://www.hackerrank.com/writing/tab-proctoring-what-it-catches-and-what-it-misses .
