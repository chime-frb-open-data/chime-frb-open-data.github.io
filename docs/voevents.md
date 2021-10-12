## CHIME/FRB Virtual Observatory Events

### Frequently Asked Questions (FAQs)

New and existing users should check this page regularly for curated answers to the most common questions regarding subscriptions to the CHIME/FRB VOEvent Service. 

#### How do I connect to the CHIME/FRB VOEvent Service?

The Service requires a (free) subscription. Subscribers must first fill out a subscription form [here](https://chime-frb.ca/voevents). 

#### I just received a VOEvent from CHIME/FRB! Is it an FRB?

Not every alert will represent a true FRB. Because alerts are published in real-time, they are only verified by a human afterwards. To check the status of a particular alert, it is best to reach out to CHIME/FRB by opening a GitHub issue [here](https://github.com/chime-frb-open-data/community/issues)

#### We found something interesting in our follow-up observations of a CHIME/FRB! What next?

We ask that you cite any CHIME/FRB VOEvents that were used to trigger your observations by their VOEvent IVORN and in addition cite the usage of the Service with the following statement: **This research has made use of the CHIME/FRB VOEvent Service."**

#### I can’t connect to the Service. How can I tell if it’s down?

If you are having problems connecting to the Service, you can try these solutions that have been ranked in order of severity/complexity.

1. Did you request a subscription?

Check your records that you have an active subscription. This would have required filling the subscription Microsoft Form which provides a receipt to the email address that filled it.

2. Did you allow time for activation?

Confirm you have waited at least 3 business days since submitting the Microsoft Form, thereby allowing enough time for CHIME/FRB to activate your subscription

3. Is your IP address valid?

Check that the IP address of the machine where you VOEvent broker is running is publicly accessible, not blocked by a firewall on the local server, and exactly matches the one you gave in the subscription form.

4. Is it a deeper problem?

Please summarize the problem in a detailed GitHub issue [here](https://github.com/chime-frb-open-data/community/issues) and be sure to include the following details in the issue.

* Your subscriber details: email address, name, and academic association you gave when filling the subscription form
* Your operating system (e.g. Linux)
* Your VOEvent broker software and version (e.g. Comet v 3.1.0)
* Screenshots or code captures of any messages that your broker reports while trying to subscribe. To obtain detailed diagnostic information, run your VOEvent broker in non-demonized mode and high verbosity - for example, see Comet documentation [here](https://comet.transientskp.org/en/stable/usage/broker.html#invoking-comet).  


#### What does the number in the `known_source_name` parameter mean?

You may see an integer reported as the name of the source in a _subsequent_ VOEvent, rather than a **TNS** name. This number is an internal event registration number for CHIME/FRB, indicating that the source is not yet public.

#### How do I credit the CHIME/FRB VOEvent service in publications/ATels?

Please be sure to use the VOEvent IVORN when referencing individual VOEvents that refer to FRBs, and include the following citation statement: **This research has made use of the CHIME/FRB VOEvent Service."**

#### How should I submit issues that I’m having with the service?

Please consult our community issue page [here](https://github.com/chime-frb-open-data/community/issues). You can search existing issues first to see if your problem, or similar, has already been solved or is being actively investigated; otherwise, please open a new issue there. 

#### Is there a way to know what other telescopes/instruments are using CHIME/FRB VOEvents for follow-up?

The following is a recent list of all observatories, telescopes, and instruments currently using CHIME/FRB VOEvents.

* _Swift_-GUANO
* Hat Creek Radio Observatory
* VERITAS

How can I contact the CHIME/FRB team for quick questions?

Please go [here](https://github.com/chime-frb-open-data/community/issues) first to see if your question has already been answered among previously submitted issues. If not, please add a new one. We are committed to addressing issues in a timely manner. 

#### Does CHIME/FRB issue retractions for spurious events?

Real-time VOEvents are verified by humans only after they have been published. Following human verification, an event may be found to be a false positive signal, for example due to RFI contamination. While the real-time FRB detection pipeline performs multiple levels of RFI excision, it is not a perfect filter. 

Under the current regime, once per day around 22:00 Pacific Standard Time we will publish _retraction_ VOEvents in bulk for all false positives classified in the previous 24 hour period.

#### How precise and accurate is the localization region in the VOEvent?

The real-time localization is reported as an on-sky circle in celestial coordinates. The precision and accurcy of this circular region is sensitive to whether the FRB was detected in one beam or multiple beams.

##### Precision

Multi-beam FRB detections typically come with the least precise real-time localization. The error radius can be as large as 1 to 2 degrees, and larger in rare instances. Typically the worst localizations are for events that later turn out to be RFI that was detected in many beams.

Single-beam FRB detections are typically circular regions with a radius equal to half the detection beam width at 600 MHz, which is about 0.5 degrees. However, it can be much better in some instances, as low as 10 arcminutes. 

##### Accuracy

The accuracy of real-time localizations has been evaluated and described in [CHIME/FRB Collaboration et al., 2019](https://arxiv.org/abs/1908.03507), using the method of pulsar analogues. 

#### Which FRB parameters are most important?

Every subscriber has constructed a potentially unique follow-up campaign. For instance, one may be interested in low dispersion measure (DM) FRBs, while another is interested in follow-up of specific known repeating FRBs. For that reason, the FRB parameters of import vary from one campaign to the next. For example, the `dm` and `snr` parameters in every VOEvent can be used with thresholds to trigger on CHIME-detected FRBs that have low DM and high-SNR, to study FRBs that are _potentially_ in the local Universe. In a different scenario, a threshold on `timestamp_utc` is important for campaigns looking to achieve very low latency follow-up. 
