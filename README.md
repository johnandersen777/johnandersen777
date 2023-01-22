# 🐢 [Rolling Alice...](https://github.com/intel/dffml/blob/alice/docs/tutorials/rolling_alice/0000_architecting_alice/README.md#rolling-alice-volume-0-introduction-and-context) ⏳

🤙 Hello Entity of the Internet! :metal: I'm [John](https://pdxjohnny.github.io/about/). 🎩

⛓️🕳 I've fallen down the open source supply chain security rabbit hole. 🌳🐇

My current focus is around leveraging threat model and architecture
information to facilitate automated context aware decentralized gamification
/ continuous improvement 🚄 of the security lifecycle / posture of open source
projects.

It'd be fun if you joined in on this adventure. 🛤️ I can promise it's
going to be a wild ride. 🛼🎢

> Source: [Living Threat Models Are Better Than Dead Threat Models](https://gist.github.com/pdxjohnny/07b8c7b4a9e05579921aa3cc8aed4866#file-rolling_alice_progress_report_0006_living_threat_models_are_better_than_dead_threat_models-md) [John L. Whiteman and John S. Andersen (aka John^2)]
>
> *The cornerstone of security for every application starts with a [threat model](https://owasp.org/www-community/Threat_Modeling_Process). Without it, how does one know what to protect and from whom? Remarkably, most applications do not have threat models, take a look at the open-source community. And, even if a threat model is created, it tends to be neglected as the project matures since any new code checked in by the development team can potentially change the threat landscape. One could say that the existing threat model is as good as dead if such a gap exists.*
>
> *Our talk is about creating a [Living Threat Model (LTM)](https://github.com/johnlwhiteman/living-threat-models) where the same best practices used in the [continuous integration](https://github.com/intel/dffml/tree/alice/docs/tutorials/rolling_alice/0000_architecting_alice#what-is-alice) of source code can aptly apply to the model itself. LTMs are machine readable text files that coexist in the Git repository and, like, source code, can be updated, scanned, peer reviewed and approved by the community in a transparent way. Wouldn’t it be nice to see a [threat model](https://github.com/johnlwhiteman/living-threat-models/blob/main/THREATS.md) included in every open-source project?*
>
> *We need to consider automation too to make this work in the CI/CD pipeline. We use the open-source [Data Flow Facilitator for Machine Learning (DFFML)](https://github.com/intel/dffml) framework to establish a [bidirectional data bridge](https://github.com/intel/dffml/blob/alice/docs/arch/0009-Open-Architecture.rst) between the LTM and source code. When a new pull request is created, an [audit-like scan](https://github.com/johnlwhiteman/living-threat-models/blob/main/demo/ALICE.rst#living-threatsmd) is initiated to check to see if the LTM needs to be updated. For example, if a scan detects that new cryptography has been added to the code, but the existing LTM doesn’t know about it, then a warning is triggered. Project teams can triage the issue to determine whether it is a false positive or not, just like source code scans.*
>
> *We have been working on this effort for a few years and feel we are on the right track to make open-source applications more secure in a way that developers can understand.*

- [johnandersenpdx@gmail.com](mailto:johnandersenpdx@gmail.com?subject=Introduction)
- [@pdxjohnny@mastodon.social](https://mastodon.social/@pdxjohnny)
- [Rolling Alice: Progress Reports](https://www.youtube.com/playlist?list=PLtzAOVTpO2jYt71umwc-ze6OmwwCIMnLw)
- [Rolling Alice: Progress Report Transcripts](https://gist.github.com/pdxjohnny/07b8c7b4a9e05579921aa3cc8aed4866)
- [Alice Engineering Comms](https://github.com/intel/dffml/discussions/1406?sort=new)
  - Come! Roll Alice with us! She's falling down the rabbit hole [too](https://github.com/intel/dffml/blob/alice/docs/tutorials/rolling_alice/0001_coach_alice/0000_introduction.md)!

[![hole-rabbit-hole](https://user-images.githubusercontent.com/5950433/196436807-68881b75-2006-4734-b4a2-63dc3d17b634.gif)](https://github.com/intel/dffml/commit/291cfbe5153414932afe446aa4f6c2e298069914)

---

![Metrics](https://metrics.lecoq.io/pdxjohnny?template=classic&base.indepth=true&repositories.batch=300&repositories.forks=true&discussions=1&repositories=1&followup=1&lines=1&gists=1&introduction=1&rss=1&achievements=1&notable=1&activity=1&languages=1&stars=1&base=header%2C%20activity%2C%20community%2C%20repositories%2C%20metadata&base.indepth=true&base.hireable=false&base.skip=false&repositories.batch=300&repositories.forks=true&repositories.affiliations=owner&languages=false&languages.limit=8&languages.threshold=0%25&languages.other=false&languages.colors=github&languages.sections=most-used&languages.indepth=false&languages.analysis.timeout=15&languages.analysis.timeout.repositories=7.5&languages.categories=markup%2C%20programming&languages.recent.categories=markup%2C%20programming&languages.recent.load=300&languages.recent.days=14&lines=false&lines.sections=base&lines.repositories.limit=4&lines.history.limit=1&stars=false&stars.limit=4&followup=false&followup.sections=repositories&followup.indepth=false&followup.archived=true&repositories=false&repositories.pinned=0&repositories.starred=4&repositories.random=0&repositories.order=featured%2C%20pinned%2C%20starred%2C%20random&discussions=false&discussions.categories=true&discussions.categories.limit=0&achievements=false&achievements.threshold=C&achievements.secrets=true&achievements.display=detailed&achievements.limit=0&notable=false&notable.from=organization&notable.repositories=true&notable.indepth=true&notable.types=commit&notable.self=true&activity=false&activity.limit=5&activity.load=300&activity.days=14&activity.visibility=all&activity.timestamps=false&activity.filter=all&gists=false&introduction=false&introduction.title=true&rss=false&rss.source=https%3A%2F%2Fmastodon.social%2F%40pdxjohnny.rss&rss.limit=5&config.timezone=America%2FLos_Angeles)
