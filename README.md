# 🐢 [Rolling Alice...](https://github.com/dffml/dffml/blob/main/docs/tutorials/rolling_alice/0000_architecting_alice/README.md#rolling-alice-volume-0-introduction-and-context) ⏳

🤙 Hello Entity of the Internet! :metal: I'm John. 🎩

⛓️🕳 I've fallen down the open source supply chain security rabbit hole. 🌳🐇

My current focus is around leveraging threat model and architecture
information to facilitate automated context aware decentralized gamification
/ continuous improvement 🚄 of the security lifecycle / posture of open source
projects. The aim is to harden train of thought security. 😜

It'd be fun if you joined in on this adventure. 🛤️ I can promise it's
going to be a wild ride. 🛼🎢

```bash
# From within TMUX
export SSH_CALLER_PATH=$(mktemp -d); export AGI_SOCK="${SSH_CALLER_PATH}/agi.sock"; export INPUT_SOCK="${SSH_CALLER_PATH}/input.sock"; export OUTPUT_SOCK="${SSH_CALLER_PATH}/text-output.sock"; export NDJSON_OUTPUT_SOCK="${SSH_CALLER_PATH}/ndjson-output.sock"; export MCP_REVERSE_PROXY_SOCK="${SSH_CALLER_PATH}/mcp-reverse-proxy.sock"; ssh -NnT -p 2222 -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -o PasswordAuthentication=no -R /tmux.sock:$(echo $TMUX | sed -e 's/,.*//g') -R "${OUTPUT_SOCK}:${OUTPUT_SOCK}" -R "${NDJSON_OUTPUT_SOCK}:${NDJSON_OUTPUT_SOCK}" -R "${MCP_REVERSE_PROXY_SOCK}:${MCP_REVERSE_PROXY_SOCK}" -R "${INPUT_SOCK}:${INPUT_SOCK}" -L "${AGI_SOCK}:${AGI_SOCK}" user@alice.chadig.com
```

[![Rolling-Alice-Architecting-Alice-A-Shell-for-a-Ghost-2024-09-02-nmap-local](https://github.com/user-attachments/assets/b301dc13-cad1-4b67-a252-03591ce4e53a)](https://asciinema.org/a/710485)

> Source: [Living Threat Models Are Better Than Dead Threat Models](https://gist.github.com/johnandersen777/07b8c7b4a9e05579921aa3cc8aed4866#file-rolling_alice_progress_report_0006_living_threat_models_are_better_than_dead_threat_models-md) [John L. Whiteman and John S. Andersen (aka John^2)]
>
> *The cornerstone of security for every application starts with a [threat model](https://owasp.org/www-community/Threat_Modeling_Process). Without it, how does one know what to protect and from whom? Remarkably, most applications do not have threat models, take a look at the open-source community. And, even if a threat model is created, it tends to be neglected as the project matures since any new code checked in by the development team can potentially change the threat landscape. One could say that the existing threat model is as good as dead if such a gap exists.*
>
> *Our talk is about creating a [Living Threat Model (LTM)](https://github.com/johnlwhiteman/living-threat-models) where the same best practices used in the [continuous integration](https://github.com/intel/dffml/tree/main/docs/tutorials/rolling_alice/0000_architecting_alice#what-is-alice) of source code can aptly apply to the model itself. LTMs are machine readable text files that coexist in the Git repository and, like, source code, can be updated, scanned, peer reviewed and approved by the community in a transparent way. Wouldn’t it be nice to see a [threat model](https://github.com/johnlwhiteman/living-threat-models/blob/main/THREATS.md) included in every open-source project?*
>
> *We need to consider automation too to make this work in the CI/CD pipeline. We use the open-source [Data Flow Facilitator for Machine Learning (DFFML)](https://github.com/dffml/dffml) framework to establish a [bidirectional data bridge](https://github.com/dffml/dffml/blob/main/docs/arch/0009-Open-Architecture.rst) between the LTM and source code. When a new pull request is created, an [audit-like scan](https://github.com/johnlwhiteman/living-threat-models/blob/main/demo/ALICE.rst#living-threatsmd) is initiated to check to see if the LTM needs to be updated. For example, if a scan detects that new cryptography has been added to the code, but the existing LTM doesn’t know about it, then a warning is triggered. Project teams can triage the issue to determine whether it is a false positive or not, just like source code scans.*
>
> *We have been working on this effort for a few years and feel we are on the right track to make open-source applications more secure in a way that developers can understand.*

- [Blog](https://johnandersen777.github.io/posts/)
- [Rolling Alice: Progress Reports](https://www.youtube.com/playlist?list=PLtzAOVTpO2jYt71umwc-ze6OmwwCIMnLw)
- [Rolling Alice: Progress Report Transcripts](https://gist.github.com/johnandersen777/07b8c7b4a9e05579921aa3cc8aed4866)
- [Alice Engineering Comms (Archive)](https://github.com/intel/dffml/discussions/1406?sort=new)
  - [Move fast, fix things](https://hbr.org/2019/01/the-era-of-move-fast-and-break-things-is-over). [Live off the land](https://www.crowdstrike.com/cybersecurity-101/living-off-the-land-attacks-lotl/). [Failure is not an option](https://github.com/dffml/dffml/tree/main/docs/tutorials/rolling_alice/0000_architecting_alice#the-scary-part).
  - Come! Roll Alice with us! She's falling down the rabbit hole [too](https://github.com/dffml/dffml/blob/main/docs/tutorials/rolling_alice/0001_coach_alice/0000_introduction.md)!

[![hole-rabbit-hole](https://user-images.githubusercontent.com/5950433/196436807-68881b75-2006-4734-b4a2-63dc3d17b634.gif)](https://johnandersen777.github.io/a_guide_to_good/)
