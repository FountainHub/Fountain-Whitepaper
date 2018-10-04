# Appendix 2: Draft Community Governance
## Proposal
Each community user can obtain a proposal by pledge FP, and “reporting” content that includes prohibited content is a common form of proposal. The purpose of pledge FP is to prevent waste of public resources caused by random proposals. The system will return the pledge of the FP after the proposal is approved and the “appeal” period is overdue, or the “appeal” proposal is completed. If the proposal fails, the pledge FP will be credited to the next day's community contributor quota pool, allocated at the next additional issue, continuing to motivate community governance in the form of FP.

## Judge
We will establish a system of judges regarding the jury system:

Judging panel screening criteria

* Before using the community officially, users should choose the language they are primarily reading and the language they can judge to avoid the situation where the content of the proposal cannot be read.
* To prevent the judge from arbitrarily misjudged, the judge needs to pledge the FP equal to the pledge of the proposal. Therefore, the judge should lock in enough FPs, such as 10000FP, in advance to pay the FP required to judge the mortgage.
* For each proposal, the judges should be randomly searched for in each FP number interval, so that the judgment is concentrated in the head users with more FP holdings or the tail users with fewer FPs.

Judging principle and process

* To improve the efficiency of community processing, the earlier the proposal is proposed, the higher the probability that the judge will see it. In principle, each proposal should be judged within 24 hours after the proposal is initiated. If the judging is not completed within 24 hours, the judgment is terminated and decided by the existing judging result (if the number of people passing through the number of participants in half).
* To prevent misjudgment, we introduced a grievance mechanism: if the proposal is approved, the sponsor (original author) will have a chance to appeal within seven days after the result of the proposal is issued, and the FP will initiate a grievance by pledge more than the original proposer...

More judges will judge appeals.

* Each proposal will only show the content of the judges and the categories of violations, and will not display the sponsor information, nor the information such as the proposal opinion or the complaint. Because we believe that the prohibited scope defined by the restricted area should be clear, in line with the judgment standards of the vast majority of ordinary users, no need to explain.
* The final result of the proposal is the result of the appeal of the proposal. If the sponsor fails to appeal within seven days, the final result is the result of the first judging.
* If the result of the proposal is consistent with the judge's judgment, the judge (including the judge of the first judgment and appeal proposal) will be returned to the judge, and this part of the FP will be counted as the judge of the day. Weights. Therefore, the pledge period of the judge may be up to nine days.
* If the final case result is inconsistent with the judge's judgment result, the FP of the initial review judge's pledge will be frozen, and the FP during the freeze period will not be included in the case income, and will not be included in the judge's FP lock.
* When the proposal is approved, the article or comment reported by the proposal will be immediately hidden.
* A hidden article or comment will no longer be able to participate in the calculation of content weights and receive corresponding revenue.
* If the appeal proposal is finally approved, the display of the article or comment will be resumed, and the Token revenue distribution will be re-acquired from the date of recovery. The Token allocation for the concealed period is not compensated.

Proposal to judge relevant parameters

| **Proposal category**   | **Pledged FP base value**   | **Number of judges required**   | **Required pass ratio**   | 
|:----|:----|:----|:----|
| proposal   | 100FP   | 9   | 6/9   | 
| "Appeal" proposal   | Original proposal pledge + 100FP   | 15   | 9/15   | 


Although we hope to adopt the above system, we hope to achieve community autonomy and effectively manage it. However, after the proposal, “it happened to be” that there were enough irresponsible judges, the probability of releasing the content of the problem still exists. To prevent this from happening, any content can be repeatedly proposed, and each proposal can also be appealed. However, FP for each proposal required pledge = the total number of proposals for the content × the proposal needs to pledge the FP base value.
## Penalty
To maintain a healthy and orderly and sustainable community ecology, we will set the penalty as:

* When an author is successfully reported for the first time and has no complaint or failure to appeal, the author enters the observation period. When the article or comment is published again during the observation period, the FP is required to be pledged as a deposit, and the pledge period of the deposit is equal to the observation period. The number of days, which means that even if the observation period is over, the deposit may still be in the pledge period.
* If the remaining FP in the user's account is insufficient to pay the deposit, the user may not post articles or comments.
* The FP during the pledge period will not participate in the distribution of all community rewards.
* If the newly published content is reported again successfully during the observation period and there is no appeal, or the appeal fails, the full deposit of the article or comment is immediately deducted.
* When the author meets the punishment conditions again, the penalty standard will also increase and meet the exponential growth rule: ![](http://latex.codecogs.com/svg.latex?%24P_n%3DP_0%5Ctimes%202%5E%7B%28n-1%29%7D%24), where ![](http://latex.codecogs.com/svg.latex?P_n) is the penalty value, which can be the number of days or margin FP; ![](http://latex.codecogs.com/svg.latex?P_0) is the penalty base, and *n* is the number of violations.

| **parameters**   | **Observation period**   | **FP value to be pledge**   | **Pledge period**   | 
|:----|:----|:----|:----|
| P’   | 7days   | 100FP   | 7days   | 
| Example:   |    |    |    | 
| 2nd violation   | 14days   | 200   | 14days   | 
| ……   | ……   | ……   | ……   | 
| 5th violation   | 112days   | 1600   | 112days   | 
| ……   | ……   | ……   | ……   | 


All pledged FPs deducted by the proposer, judge, and even the author will be credited to the community contributor quota pool, allocated at the next additional issue, and distributed in the form of FP.

Although there are many doubts about the efficiency of decentralized governance, we are willing to work with like-minded friends to manage the entire ecology, and to discuss and optimize the governance program. For those interested in this topic, please send an email to: pilot@fountainhub.com
