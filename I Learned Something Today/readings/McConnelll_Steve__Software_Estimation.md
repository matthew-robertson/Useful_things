Impression:



## Summarizing takeaways:
1. Targets are your objectives. Commitments are targets with deadlines. Neither is the same as your estimate. The goal of your estimate is to be accurate, not to get a specific result. If the two diverge, your solution can't be to massage the final estimate. You can 1) Remove targets, 2) extend commitment deadlines, or 3) adjust the inputs to your estimate. Developers have a nasty habit of "optimistic" estimation, which will often be made further optimistic as it travels up the chain. "Before we can make our estimates more accurate, we need to make them *bigger*."
1. Tip #54 is a big one: "Do not address estimation uncertainty by biasing the estimate. Address uncertainty by expressing the estimate in uncertain terms."
1. No estimate is is 100% accurate, so don't provide single-point estimates if you can avoid it. Being explicit about your confidence level is good, making it up isn't. Single-point estimates tend to look more like best-case estimates than average case ones. Similarly, worst-case estimates have a nasty habit of being "optimistic worst-case" rather than true worst case.
1. Simply don't give off the cuff estimates. People are *really*, *really* bad at them.
1. The Cone of Uncertainty is an important tool to keep in mind in costing. tl;dr - the less defined the project is, the less confident your estimates can possibly be. For example: Before requirements are complete, data indicates the best range you can get is 0.67 - 1.50X the nominal result. Note: the cone doesn't narrow just because time has passed, you need to actually work to narrow it.
1. Project managers are there to help you hit your deadlines, but you can only manage so much project. Anecdata indicates a project can be landed by skilled manages if the estimates are within ~20% of the real timeline. One key benefit of estimating (and re-estimating) is in learning that you're going to miss *early*, while those levers are the most useful.
1. About a quarter of all projects are finished late, another quarter abandoned entirely. The rest are either late, over budget, or both.
1. Your estimates only include the work you're estimating. We're *really* good at forgetting to account for all the time spent reviewing the work of others, in meetings, etc.... One way to account for this is to base your estimates on historical data, which can include all that extra work.
1. Ideally, individual tasks should be decomposed such that the pieces take <3 days of work. Larger, and there's too much uncertainty.
1. Data's important to estimating&planning well. Historical data's invaluable, and is best captured *during* the work rather than after. In addition, it's important to review your initial estimates after the fact to compare how they related to the true results. Ideally, your estimates will under-estimate as often as they over-estimate.
1. Schedules are only so compressible (or so expandable). Above that compression maxima you hit the Impossible Zone. Estimates are that schedules can't be compressed below 75% of nominal. Not by being cleverer, not by working harder or with more people. You just can't do it.
1. "The fact that an external requirement exists does not necessarily mean it's possible to meet that requirement. It does mean that you need to make it perfects clear to the executives you're dealing with that you understand the requirement and you take it seriously."

###  Estimation Techniques
1. Timelines are pretty heavily correlated with project size, and simple methods are often better than complex ones. I.E.: Find something that correlates with project size, and count that something to derive estimates. Ideally, the thing you're counting will have at least 20 items to be statistically significant.
1. You can create a rough estimate of the expected case by calculating `[best_case + 4*most_likely_case + worst_case]/6`.
1. `StandardDeviation = SumOfWorstCaseEstimates - SumOfBestCaseEstimates) / 6`
1. "As statistically valid 75%-likely estimate would be `ExpectedCase + 0.67*StandardDeviation`".
1. "Tip #58 - Exercise caution when calculating estimates that use numeric ratings scales. Be sure that the numeric categories in the scale actually work like numbers, not like verbal categories such as small, medium, and large."
1. "If the group agrees on a single estimate without much discussion, the coordinator assigns someone to play devil's advocate."
1. If the group *doesn't* agree on a single estimate without much discussion, you're better off having everyone re-vote rather than averaging the results or something. In the author's study, ~20% of groups initial estimation ranges didn't include the "true" answer, but their final estimates did. Similarly, ~66% of groups got a more accurate estimate by re-voting than by averaging.
1. It's important to re-estimate, and to re-estimate at planned times (and/or: as soon as requirements change). Getting stuck re-estimating after slippage late in the project is no fun for anyone. You *should* use more accurate estimation methods as time goes on.
1. You want to use multiple estimation methods when possible. In doing so, estimates that converge are a good sign you're on the right track. Estimates that disagree are a sign something weird's going on and needs to be looked into.
1. Function Points are a decent way to estimate because they're correlated with size. TL;DR - Calculate function points as `External_Inputs * 4 + External Outputs *5 + External Queries*4 + Internal Logic files*10 + External Interface files*7`. As a simple first-order estimate, take that total and raise it to the power of 0.37. The ISBSG method is `staffMonths = 0.512*FunctionPoints^0.392 * MaximumTeamSize^0.791`
1. Once you have an estimate in staff months, you need ot turn it into calendar months, but note that it's not as simple as just dividing by the number of devs in the team. More devs is more co-ordination&communication. 9 women in one month, and all that. A rule of thumb is `ScheduleInMonths = 3.0*staffMonths^(1/3)`, though that assumes you can modify the team-size to match.


## Quotes/Annotations:
1. "A *target* is a statement of a desirable business objective."
1. "A *commitment* is a promise to deliver defined functionality at a specific level of quality by a certain date."
1. "Do not assume that the commitment has to be the same as the estimate; it doesn't."
1. "Estimation should be treated as an unbiased, analytical process; planning should be treated as a biased, goal-seeking progress. With estimation... the goal is accuracy; the goal is not to seek a particular result. But the goal of planning is to seek a particular result."
1. "Both estimation and planning are important, but the fundamental differences between the two activities mean that combining the two tends to lead to poor estimates *and* poor plans."
1. "Tip #2 - When you're asked to provide an estimate, determine whether you're supposed to be estimating or figuring out how to hit a target."
1. "Tip #4 - When you see a single-point estimate, that number's probability is not 100%. Ask what the probability of that number is."
1. "all estimates include a probability, whether the probability is stated or implied. An explicitly stated probability is one sign of a good estimate."
1. "In 1986, Professors S.D. Conte, H.E. Dundsmore, and V.Y. Shen proposed that a good estimation approach should provide estimates that are withing 25% of the actual results 75% of the time."
1. "Events that happen during the project nearly always invalidate the assumptions that were used to estimate the project in the first place... It becomes impossible to make a clean analytical assessment of whether the project was estimated accurately because the software project that was ultimately delivered is not the project that was originally estimated."
1. "The primary purpose of a software estimation is not to predict a project's outcome; it is to determine whether a project's targets are realistic enough to allow the project to be controlled to meet them."
1. "I have found that if the initial target and initial estimate are within about 20% of each other, the project manager will have enough maneuvering room to control the feature set, schedule, team size, and other parameters to meet the projects business goals; other experts concur."
1. "A good estimate is an estimate that provides a clear enough view of the project reality to allow the project leadership to make good decisions about how to control the project to hit its targets."
1. "No one has ever gotten [every question on the 'How Good an Estimator Are You? quiz] correct. I've concluded that most people's intuitive sense of '90% confident' is really comparable to something closer to '30% confident. Other studies have confirmed this basic finding... I've concluded that specific percentages such as '90%' are not meaningful unless they are supported through some kind of statistical analysis."
1. "Tip #6 - Avoid using artificially narrow ranges. Be sure the ranges you use in your estimates don't misrepresent your confidence in your estimates."
1. "[The common definition of estimate is] 'the most optimistic prediction that has a non-zero probability of coming true.' ... Accepting this definition leads irrevocably toward a method called what's-the-earliest-date-by-which-you-can't-prove-you-won't-be-finished estimating." - Tom DeMarco
1. "Managers and other project stakeholders sometimes fear that, if a project is overestimated, Parkinson's Law will kick in - the idea that work will expand to fill available time."
1. "Developers typically estimate 20% to 30% lower than their actual effort. Merely using their normal estimates makes the project plans optimistic."
1. "Once a project gets into 'late' status, project teams engage in numerous activities that they don't need to engage in during an 'on-time' project: More status meetings..., Frequent reestimation..., Apologizing to key customers for missing delivery dates... The important characteristic of each of these activities is that they don't need to occur *at all* when a project is meeting its goals."
1. "In software, the penalty for overestimation is *linear* and *bounded*... but the penalty for underestimation is *nonlinear* and *unbounded*."
1. "What's notable about The Standish Group's data is that it doesn't even have a category for early delivery! The best possible performance is meeting expectations 'on time/on budget' - and the other options are all downhill from there."
1. "Jones has observed for many years that project success depends on project size. That is, larger projects struggle more than smaller projects do."
1. "Overall... about one quarter of all projects are delivered on time; about one quarter are canceled; and about half are delivered late, over budget, or both."
1. "Software does not have a neutral estimation problem. The industry data shows clearly that the software industry has an underestimation problem. Before we can make our estimates more accurate, we need to start making the estimates *bigger*."
1."One of the best ways to track progress is to compare planned progress with actual progress.... If the planned progress is fantasy, a project typically begins to run without paying much attention to its plan and it soon becomes meaningless to compare actual progress with planned progress."
1. "About 40% of all software errors have been found to be caused by stress."
1. "When schedule pressure is extreme, about four times as many defects are reported in the released software as are reported for software developed under less extreme pressure."
1. "A project team that holds its ground and insists on an accurate estimate will improve its credibility within its organization"
1. "The detection of a mismatch between the project goal and the project estimate should be interpreted as incredibly useful, incredibly rare, early-in-the-project risk information. The mismatch indicates a substantial chance that the project will fail to meet its business objective. Detected early, numerous corrective actions are available, and many of them are high leverage.... if this mismatch is allowed to persist, the options that will be available for corrective action will be far fewer and will be much lower leverage."
1. "Tip #10 - Many businesses value predictability more than development time, cost, or flexibility. Be sure you understand what your business values the most."
1. "Estimation error creeps into estimates from four generic sources: inaccurate information about the project being estimated, inaccurate information about the capabilities of the organization that will perform the project, too much chaos in the project to support accurate estimation, inaccuracies arising from the estimation process itself."
1. "One question that managers and customers ask is 'If I give you another week to work on your estimate, can you refine it so that it contains less uncertainty?' ... The reason the estimate contains variability is that the software project itself contains variability. The only way to reduce the variability in the estimate is to reduce the variability in the project."
1. "The Cone of Uncertainty represents the *best-case accuracy* that is possible to have in software estimates at different points in a project... It isn't possible to be more accurate; it's only possible to be more lucky."
1. "Estimates can fail to improve... The issue isn't really that the estimates don't converge, the issue is that the project itself doesn't converge."
1. "Tip #12 - Don't assume that the Cone of Uncertainty will narrow itself. You must force the Cone to narrow by removing sources of variability from your project."
1. "This tendency to use ranges that are too narrow can be addressed two ways. The first is to start with a 'most likely' estimate and then compute the ranges using predefined multipliers"

  | Phase                       | Possible Error on Low Side | Possible Error on High Side |
  | --------------------------- | -------------------------- | --------------------------- |
  | Initial Concept             | 0.25x (-75%)               | 4.0x (+300%) |
  | Approved Product Definition | 0.50x (-50%)               | 2.0x (+100%) |
  | Requirements Complete       | 0.67x (-33%)               | 1.5x (+50%) |
  | UI Design Complete          | 0.80x (-20%)               | 1.25x (+25%) |
  | Detailed Design Complete    | 0.90x (-10%)               | 1.10x (+10%) |

1. "Tip #14 - Account for the Cone of Uncertainty by having one person create the 'how much' part of the estimate and a different person create the 'how uncertain' part of the estimate."
1. "Software organizations routinely sabotage their own projects by making commitments too early in the Cone of Uncertainty. If you commit at Initial Concept of Product Definition time, you will have a factor of 2x to 5x error in your estimates.... A skilled project manager can navigate a project to completion if the estimate is within about 20% of the project reality."
1. "Common examples of project chaos include: requirements that weren't investigated very well in the first place, lack of end-user involvement in requirements validation, poor designs that lead to numerous errors in the code, poor coding practices that give rise to extensive bug fixing, inexperienced personnel, incomplete or unskilled project planning, prima donna team members, abandoning planning under pressure, developer gold-plating, lack of automated source code control."
1. "Tip #15 - ... You can't accurately estimate an out-of-control process. As a first step, fixing the chaos is more important than improving the estimates."
1. "Requirements changes are often not tracked and the project is often not reestimated when it should be. In a well-run project, and initial set of requirements will be baselined.... As new requirements are added or old requirements are revised,... estimates will be modified to reflect those changes."
1. "Tip #17 - Include time in your estimates for stated requirements, implied requirements, and nonfunctional requirements - that is, *all* requirements. Nothing can be build for free, and your estimates shouldn't imply that it can."
1. "Software-Development Activities Commonly Missing from Software Estimates: mentoring of new team members, requirements clarifications, participation in technical reviews, review of technical documentation, ...."
1. "A study of about 100 schedule estimates within the U.S. Department of Defense found a consistent 'fantasy factor' of about 1.33.... Common variations on this optimism theme include the following: 'we'll be more productive on this project than we were on the last project; a lot of things went wrong on the last project, not so many things will go wrong on this project; we started the project slowly... all the lessons we learned will allow us to finish the project much faster than we started it."
1. "Developers present estimates that are optimistic. Executives like the optimistic estimates because they imply that desirable business targets are achievable. Managers like the estimates because they imply that they can support upper management's objectives. And so the software project is off and running with no one ever taking a critical look at whether the estimates were well founded in the first place."
1. "...the response of customers and managers when they discover that the estimate does not align with the business target is sometimes to apply more pressure to the estimate, to the project, and to the project team. Excessive schedule pressure occurs in 75% to 100% of large projects."
1. "One of the most enduring and useful conclusions from research on forecasting is that simple methods are generally as accurate as complex methods."
1. "I've found that the safest policy is not to give off-the-cuff estimates. ... The average off-the-cuff estimate has a mean magnitude of relative error of 67%, whereas the average reviewed estimate has an error of only 30% - less than half. (These are not software estimates, so the percentage errors shouldn't be applied literally to software project estimates."
1. "Using an estimate of 395.7 days instead of 1 year is like representing pi as 3.37882 - the number is more precise, but it's really less accurate."
1. "software size being the largest cost driver might seem obvious, yet organizations routinely violate this fundamental fact in two ways: costs, effort, and schedule are estimated without knowing how big the software will be; costs, effort, and schedule are not adjusted when the size of the software is consciously increased"
1. "Software that is developed with the goal of later reuse can increase costs as much as 31%. This doesn't say whether the initiative actually succeeds. Industry experience has been that forward-looking reuse programs often fail."
1. "What this means from a project planning and control point of view is that small and medium-sized projects can succeed largely on the basis of strong individuals. Large projects still need strong individuals, but how well the project is managed, how mature the organization is, and how well the individuals coalesce into a team become significant."
1. "One of the secrets of this book is that you should avoid doing what we traditionally think of as estimating! If you can *count* the answer directly, you should do that first."
1. "Tip #30 - Count if at all possible. Compute when you can't count. Use judgement alone only as a last resort."
1. "Find something to count that's highly correlated with the size of the software you're estimating."
1. "Find something to count that will produce a statistically meaningful average.... Statistically, you need a sample of at least 20 items for the average to be meaningful."
1. "Historical and project data are both tremendously useful and can support creation of highly accurate estimates. Industry data is a temporary backup that can be useful when you don't have historical data or project data."
1. "historical data accounts for a raft of organization influences that affect project outcomes."
1. "Tip #36 - Use historical data to help avoid politically charged estimation discussions arising from assumptions like 'my team is below average.'"
1. "If you're not already collecting historical data, you can start with a very small set of data: Size, Effort, Time, and Defects."
1. "it doesn't particularly matter how you answer these questions. What does matter is that you answer these questions consistently across projects so that whatever assumptions are baked into the data you collected is *consciously* projected forward in your estimates."
1. "Historical data tends to be easiest to collect if it's collected while the project is underway. It's difficult to go back six months after a project has been completed and reconstruct the 'fuzzy front end' of the project to determine when the project began. It's also easy to forget how much overtime people worked at the end of the project."
1. "Tip #39 - As a project is underway, collect historical data on a periodic basis so that you can build a data-based profile of how your projects run."
1. "A review of studies on estimation accuracy found that in studies in which estimation models were not calibrated to the estimation environment, expert estimates were more accurate than the models. But the studies that used models calibrated with historical data found that the models were as good as or better than the expert estimates."
1. "Magne Jorgensen reports that increased experience in the activity being estimated does not lead to increased accuracy in the estimates for the activities."
1. "When estimating at the task level, decompose estimates into tasks that will require no more than about 2 days of effort. Tasks larger than that will contain too many places that unexpected work can hide."
1. "People's worst cases are often optimistic worst cases rather than true worst cases."
1. "single-point estimates tend to be akin to Best Case estimates."
1. `expected_case = [best_case + (4*most_likely_case) + worst_case]/6`, as people's estimates tend to be optimistic, you can swap weights to 1,3,2 in the short term.
1. Checklist for individual estimates
    1. Is what's being estimated clearly defined?
    1. Does the estimate include all the *kinds of work* needed to complete the task?
    1. Does the estimate include all the *functionality areas* needed to complete the task?
    1. Is the estimate broken down into enough detail to expose hidden work?
    1. Did you look at documented facts (written notes) from past work rather than estimating purely from memory?
    1. Is the estimate approved by the person who will actually do the work?
    1. Is the productivity assumed in the estimate similar to what has been achieved on similar assignments?
    1. Does the estimate include a Best Case, Worst Case, and Most Likely Case?
    1. Is the Worst Case really the worst case? Does it need to be made even worse?
    1. Is the Expected Case computed appropriately from the other cases?
    1. Have the assumptions in the estimate been documented?
    1. Has the situation changed since the estimate was prepared?
1. "Tip #45 - Compare actual performance to estimated performance so that you can improve you individual estimates over time."
1. "I've worked with companies that have their developers report on their actual results versus their estimates at a Monday morning standup meeting. This reinforces the idea that accurate estimates are an organizational priority."
1. "If you can't use the sum of the best and worst cases to produce overall Best Case and Worst Case estimates, what do you do? A common approximation in statistics is to assume that 1/6 of the range between a minimum and a maximum approximately equals one standard deviation. This is based on the assumption that the minimum is only 0.135% likely and the maximum includes 99.86% of all possible values."
1. `StandardDeviation = SumOfWorstCaseEstimates - SumOfBestCaseEstimates) / 6`
1. "As statistically valid 75%-likely estimate would be `ExpectedCase + 0.67*StandardDeviation`".
1. "A realistic approach to computing standard deviation from best and worst cases is to divide each individual range by a number that's closer to 2 than 6. Statistically, dividing by 2 implies that the estimator's ranges will include the actual outcome 68% of the time, which is a goal that can be achieved with practice."
1. "Tip #52 - Focus on making you expected case estimates accurate.... If the individual estimates are not accurate, aggregation will be problematic"
1.  "Tip #53 - Estimate new projects by comparing them to similar past projects, preferably *decomposing the estimate into at least five pieces*"
1. "In this case, we'll just assume they're weighted equally. *We should document this assumption so that we can retrace our steps later*"
1. "The estimate you compute and the estimate you present are two different matters. In this computation you ended up with a single-point estimate. When you present the estimate, you might well decide to present it as a range"
1. "The best estimation response to uncertainty is not to bias the estimate but to be sure that the estimate accurately expresses any underlying uncertainty... perhaps you would fudge the uncertainty number to something like +25%, -10%."
1. "If there is a 50% uncertainty in the business rules, the estimator might apply that uncertainty to the whole estimate, rather than just to the quarter of the estimate related to business rules."
1. "Tip #54 - Do not address estimation uncertainty be biasing the estimate. Address uncertainty by expressing the estimate in uncertain terms."
1. "Accomplishing this level of accuracy requires great discipline be exercised in assigning story points to stories. It also requires checking actual project data to ensure that the ratios that are estimated are the ratios actually found in practice."
1. "Tip #58 - Exercise caution when calculating estimates that use numeric ratings scales. Be sure that the numeric categories in the scale actually work like numbers, not like verbal categories such as small, medium, and large."
1. "In [t-shirt sizing], the developers classify each feature's size relative to other features as Small, Medium, Large, or Extra Large. In parallel, the customer, marketing, sales, or other nontechnical stakeholders classify each feature's business value on the same scale. These two sets of entries are then combined." -The benefit is getting extremely quick yes and no answers early on in a project.
1. "If the group agrees on a single estimate without much discussion, the coordinator assigns someone to play devil's advocate."
1. "My experience with Wideband Delphi suggests that it cuts estimation error by an average of approximately 40% compared to the initial group average. Of the groups in my study, about two-thirds produced a more accurate answer by using Wideband Delphi than by simply averaging their estimates."
1. "20% of the groups' initial estimation ranges do not include the correct answer. This means that averaging their initial estimates cannot possibly produce an accurate result."
1. "The most sophisticated commercial software producers tend to use at least three different estimating approaches and then look for convergence or spread among their estimates."
1. "On poorly estimated projects, estimation focuses on directly estimating cost, effort, and schedule... Projects are re-estimated many times, but usually in response to schedule slippages late in the project."
1. "If the estimation inputs and process are well-defined, arbitrarily changing the output is not a rational action. Project stakeholders might not like the output, but the appropriate corrective action is to adjust the inputs and to recalculate the outputs, not just to change the output to a different answer."
1. "Within the estimation procedure, the only factor that ever needs to be estimated in the traditional sense (that is, by using judgment) is size. And you'll need to use judgement to estimate size only very early in a project, before you have requirements, stories, use cases, or something else you can count. Effort is computed from the size estimate by using historical productivity data. Schedule, cost, and features are computed from the effort estimate."
1. "Because of the Cone of Uncertainty, most projects will benefit from being re-estimated several times. Estimates created in the later days of a project can be more accurate than estimates created earlier. Project plans and controls can then be tightened up when the project is re-estimated with better accuracy."
1. "Re-estimation does not consist of simply doing the same estimation work again. It consists of converting to more accurate approaches as the project progresses."
1. "As soon as you know the specific people who will be working on your project and can start handing out specific task assignments, it's time to switch from large-grain algorithmic approaches to bottom-up approaches..."
1. "It's unlikely that the whole estimate is accurate, except for the part that you've had real experience with.... Tip #74 - When you re-estimate in response to a missed deadline, base the new estimate on the project's actual progress, not on the project's planned progress."
1. "There are no magic times to re-estimate.... Projects commonly re-estimate at major milestones, upon major releases, or when major project assumptions change"
1. "Tip #77 - Develop a Standardized Estimation Procedure at the organizational level; use it at the project level."
1. Typical Correspondence between SDLC Gates and Estimates

  | SDLC Gate| Estimate Accuracy (for remainder of project) | Estimate usage |
  | ---------- | ---------- | -------- |
  | 1 | -75%%, +300% | Vision Estimate. For internal use only, do not publish outside of development group.|
  | 2 | -50%, +100% | Exploratory Estimate. For internal company use only, do not publish externally |
  | 3 | -20%, +25% | Budget Estimate. OK to publish the high end of the range externally. Do not publish the lower number or midpoint |
  | 4 | -10%, +10% | Final Commitment Estimate. OK to publish the midpoint number externally. |

1. "Task nominals should be computed using the formula `[BestCast + (4 * MostLikelyCase) + WorstCase] / 6`"
1. "Compare [budget estimate] to [preliminary dev estimate]. Compute a nominal estimate, `N`, using the following formula: `(2 * TheHigherEstimate + TheLowerEstimate) / 3)`"
1. "After each project, you should assess the effectiveness of your estimates in several ways:
  1. How accurate were your estimates? Did your ranges include the final result...?
  1. Were your ranges wide enough? Could they be made narrower and still account for the variability that you've observed?
  1. Did your estimates tend to be on the low side or the high side, or was the error tendency neutral?
1. "The number of function points in a program is based on the number and complexity of each of the following items: External Inputs, External Outputs, External Queries, Internal Logical Files, and External Interface Files"

  | Program Characteristic   | Low Complexity | Medium Complexity | High Complexity |
  | -------------------------- | ---------------- | ------------------- | -----------------|
  | External Inputs          |     _ x 3      |       _ x 4       |      _ x 6      |
  | External Outputs         |     _ x 4      |       _ x 5       |      _ x 7      |
  | External Queries         |     _ x 3      |       _ x 4       |      _ x 6      |
  | Internal Logical Files   |     _ x 4      |       _ x 10      |      _ x 15     |
  | External Interface Files |     _ x 5      |       _ x 7       |      _ x 10     |

1. "Two studies have found that Unadjusted Function Points are more strongly correlated with ultimate size than Adjusted Function Points are. Some experts also recommend eliminating the 'low complexity' and 'high complexity' judgments, and classifying all counted items as 'medium', which eliminates another source of subjectivity."
1. "As an alternative to counting function points directly, you might count GUI elements instead. This is an example of proxy-based estimation... Some uncertainly likely exists in your original counts of the number of GUI elements or your estimates of them. You introduce additional uncertainty when you convert from GUI elements to function points. And you will introduce still more uncertainty when you convert from function points to lines of code."
1. "Tip #84 - With better estimation methods, the size estimate becomes the foundation of all other estimates. The size of the system you're building is the single largest cost driver. Use multiple size-estimation techniques to make your size estimate accurate."
1. "The largest influence on a project's effort is the size of the software being build. The second largest influence is your organization's productivity."
1. Why use historical data to create estimates? "...it includes whatever effort is included in the historical data. ...If the historical data also included effort for requirements, project management, and user documentation, that's what the estimate includes."
1. "If you don't have your own historical data and are using these graphs, that's a sign that your development organization is at best average."
1. ISBSG estimation method for general projects, calibrated on about 600 projects of different types&sizes: `StaffMonths = 0.512 * FucntionPoints&0.392 * MaximumTeamSize^0.791`
1. "An interesting aspect of the ISBSG method is that the formulas for effort depend on the maximum size of the project team, with smaller teams producing smaller total estimates." Why? Probably because of increased communication channels?
1. "Tip #88 - Not all estimation methods are equal. When looking for convergence or spread among estimates, give more weight to the techniques that tend to produce the most accurate results."
1. "A rule of thumb is that you can estimate schedule early in a project using the Basic Schedule Equation: `ScheduleInMonths = 3.0 & StaffMonths^(1/3)`... the basic idea that schedule is a cube-root function of effort is almost universally accepted by estimation experts."
1. "The schedule equation implicitly assumes that you're able to adjust the team size to suit the size implied by the equation."
1. "First-Order Estimation Practice... take your function-point total and raise it to the appropriate power selected from Table 20-3."

  |                                Kind of software                                  | Better | Average | Worse |
  | ---------------------------------------------------------------------------------- | -------- | --------- | ------- |
  | Object-oriented software                                                         | 0.33   | 0.36    | 0.39  |
  | Client-server software                                                           | 0.34   | 0.37    | 0.40  |
  | Business systems, internal intranet systems                                      | 0.36   | 0.39    | 0.42  |
  | Shrink-wrapped, scientific systems, engineering systems, public internet systems | 0.37   | 0.40    | 0.43  |
  | Embedded systems, telecommunications, device drivers, systems software           | 0.38   | 0.41    | 0.44  |

1. "If the nominal schedule is 12 months with a team of 7 developers, you can't just use 12 developers to reduce the schedule to 7 months. Shorter schedules require more effort for several reasons: 1) Larger teams require more coordination and management overhead. 2) Larger teams introduce more communication paths... 3) Shorter schedules require more work to be done in parallel."
1. "Tip #92 - So not shorten a schedule estimate without increasing the effort estimate."
1. "There is an Impossible Zone, and you can't beat it... The consensus of researchers is that schedule compression of more than 25% from nominal is not possible. ...for a project of a particular size, there's a point beyond which the development schedule simply can't be shortened. Not by working harder. Not by working smarter. And not by finding creative solutions or by making the team larger. It simply can't be done (Symons 1991, Boehm 2000, Putnam and Myers 2003)"
1. "For an extended schedule to reduce effort, you must actually reduce the team size. If you simply allocate the same people to the same project fractionally instead of reducing the number of people on the team, you'll likely make matters worse instead of better"
1. Recommended tradeoffs between effort and schedule

  | Schedule compression/expansion | Effort increase/reduction |
  | ------------------------------ | ------------------------- |
  | -15%    | +100% |
  | -10%    | +50%  |
  | -5%     | +25%  |
  | Nominal | 0%    |
  | +10%    | -30%  |
  | +20%    | -50%  |
  | +30%    | -65%  |
  | >+30%   | Not practical |

1. "Mike Cohn describes the difference between ideal time and planned time as akin to the difference between the minutes on the game clock vs the minutes on the wall clock in an American football game. A normal game of American football lasts 60 minutes on the game clock. On the wall clock, a game can last anywhere from 2 to 4 hours."
1. "If you work in a large, established organization in which you have frequent corporate-overhead meetings and most people work about 40 hours per week, you might need to assume only 20 to 30 hours of project-focused time, and those hours might be spread across 2 or more projects."
1. "Capers Jones reports that, on average, technical workers apply about 6 hours of focused project time per day to their assigned projects"
1. "One factor that contributes to software's diseconomy of scale is that larger projects tend to produce more defects per line of code, which requires more defect-correction effort, which in turn drives up project costs."
1. "Lawrence Putnam provides two additional rules of thumb for defect removal. If you want to move from 95% reliability to 99% reliability, you should plan to add 25% to the 'main build' part of your schedule. You should plan to add another 95% to your schedule to improve from 99% to 99.9% reliability."
1. "Total Risk Exposure makes a good place to begin quantitative buffer planning. If you want more certainty that you will deliver on time, you should plan for a buffer that's larger than the total RE."
1. Other rules of Thumb: 1) For administrative and clerical support, add 5% to 10% to the base effort. 2) For configuration management/build support at 2% to 8% to the base effort estimate. 3) Allow for 1% to 4% increase in requirements per month. 4) To go from one-company, one-campus development to multiple-company, multiple-city development, allow for a 25% increase in effort. 5) To go from one-company, one-campus development to international outsource development, allow for a 40% increase in effort. 6) For first time development with new language and tools compared to comparable development with familiar language and tools, allow for 20% to 40% increase in effort.
1. "An essential practice in presenting an estimate is to document the assumptions embodied in the estimate."
1. "A weakness of the plus-or-minus style is that, as the estimate is passed through the organization, it tends to get stripped down to just the core estimate. Occasionally, managers simplify such an estimate out of a desire to ignore the variability implied by the estimate. More often, they simplify the estimate because their manager or their corporate budgeting system can handle only estimates that are expressed as single-point numbers. If you use this technique, be sure you can live with the single-point number that's left after the estimate gets converted to a simplified form."
1. "Should you present the full range or only the part of the range from the nominal estimate to the top end of the range?" Projects rarely become smaller over time, and estimates tend to err on the low side."
1. "Project stakeholders might think that presenting an estimate as a wide range makes the estimate useless. What's really happening is that presentation of the estimate as a wide range accurately conveys the fact that the estimate *is* useless! It isn't the presentation that makes the estimate useless; it's the uncertainty in the estimate itself."
1. "The fact that an external requirement exists does not necessarily mean it's possible to meet that requirement. It does mean that you need to make it perfects clear to the executives you're dealing with that you understand the requirement and you take it seriously."
