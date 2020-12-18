
[![Gitter](https://badges.gitter.im/chan-sccp/PAMI.svg)](https://gitter.im/chan-sccp/PAMI?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)
<!--
[![License](https://poser.pugx.org/marcelog/PAMI/license)](https://packagist.org/packages/marcelog/PAMI)
[![Latest Stable Version](https://poser.pugx.org/marcelog/PAMI/v/stable)](https://packagist.org/packages/marcelog/PAMI)
[![Documentation Status](https://readthedocs.org/projects/pami/badge/?version=latest)](http://pami.readthedocs.org/en/latest/?badge=latest)
-->
[![Build Status](https://travis-ci.org/chan-sccp/PAMI.svg)](https://travis-ci.org/chan-sccp/PAMI)
[![Coverage Status](https://coveralls.io/repos/github/chan-sccp/PAMI/badge.svg?branch=master)](https://coveralls.io/github/chan-sccp/PAMI?branch=master)
[![Code Climate](https://codeclimate.com/github/chan-sccp/PAMI/badges/gpa.svg)](https://codeclimate.com/github/chan-sccp/PAMI)
<!--
[![Issue Count](https://codeclimate.com/github/chan-sccp/PAMI/badges/issue_count.svg)](https://codeclimate.com/github/chan-sccp/PAMI)
[![Click here to lend your support to: PAMI and make a donation at pledgie.com !](https://pledgie.com/campaigns/30944.png?skin_name=chrome' border='0')](https://pledgie.com/campaigns/30944)
-->
# Fork !
Note that this is a fork of the official release by [dkgroot](https://github.com/chan-sccp/PAMI).


# Fork !
Note that this is a fork of the official release by [Marcelo Gornstein](https://github.com/marcelog/PAMI), which has not been maintained for a while (hence the fork).

# Introduction

PAMI means PHP Asterisk Manager Interface. As its name suggests its just a
set of php classes that will let you issue commands to an ami and/or receive
events, using an observer-listener pattern.

The idea behind this, is to easily implement operator consoles, monitors, etc.
either via SOA or ajax.

 * A port for nodejs is available at: http://marcelog.github.com/Nami
 * A port for erlang is available at: https://github.com/marcelog/erlami

# Resources

 * [API](doc/api.md) in Markdown
 * [API](https://chan-sccp.github.io/PAMI) in html
 * Collection of [Examples](doc/examples/)
 * [Complete PAGI/PAMI talk for the PHP Conference Argentina 2013](http://www.slideshare.net/mgornstein/phpconf-2013). Check the slide notes for the complete text :)

# PHP Versions

Note: PAMI Requires PHP 5.6+.

# Installing
Add this library to your [Composer](https://packagist.org/) configuration. In
composer.json:
```json
  "require": {
    "marayshi/pami": "*"
  }
```

# QuickStart

For an in-depth tutorial: http://marcelog.github.com/articles/pami_introduction_tutorial_how_to_install.html

```php
// Make sure you include the composer autoload.
require __DIR__ . '/vendor/autoload.php';

$options = array(
    'host' => '2.3.4.5',
    'scheme' => 'tcp://',
    'port' => 9999,
    'username' => 'asd',
    'secret' => 'asd',
    'connect_timeout' => 10,
    'read_timeout' => 10
);
$client = new \PAMI\Client\Impl\ClientImpl($options);

// Registering a closure
$client->registerEventListener(function ($event) {
});

// Register a specific method of an object for event listening
$client->registerEventListener(array($listener, 'handle'));

// Register an IEventListener:
$client->registerEventListener($listener);
```

# Using Predicates
A second (optional) argument can be used when registering the event listener: a
closure that will be evaluated before calling the callback. The callback will
be called only if this predicate returns true:

```php
use PAMI\Message\Event\DialEvent;

$client->registerEventListener(
    array($listener, 'handleDialStart'),
    function ($event) {
        return $event instanceof DialEvent && $event->getSubEvent() == 'Begin';
    })
);
```

# Example

Please see docs/examples/quickstart/example.php for a very basic example.

AsterTrace is a full application: https://github.com/marcelog/AsterTrace.

Also, you might want to look at this article: http://marcelog.github.com/articles/php_asterisk_listener_example_using_pami_and_ding.html

For an example of using asynchronous AGI with PAMI, see docs/examples/asyncagi

The [march edition](http://sdjournal.org/a-practical-introduction-to-functional-programming-with-php-sdj-issue-released/) of [Software Developer Journal](http://sdjournal.org/) features a complete article about writing telephony applications with PAMI and PAGI.

# Currently Supported Events

More events will be added with time. I can only add the ones I can test for and
use, so your contributions may make the difference! ;)

Unknown (not yet implemented) events will be reported as UnknownEvent, so you
can still catch them. If you catch one of these, please report it!

 * AgentCalled
 * AgentComplete
 * AgentConnect
 * AgentDump
 * Agentlogin
 * Agentlogoff
 * AgentRingNoAnswer
 * Agents
 * AGIExec
 * AGIExecEnd
 * AGIExecStart
 * Alarm
 * AlarmClear
 * AorDetail
 * AorList
 * AorListComplete
 * AsyncAGI
 * AsyncAGIEnd
 * AsyncAGIExec
 * AsyncAGIStart
 * AttendedTransfer
 * AuthDetail
 * AuthList
 * AuthListComplete
 * AuthMethodNotAllowed
 * BlindTransfer
 * Bridge
 * BridgeCreate
 * BridgeDestroy
 * BridgeEnter
 * BridgeInfoChannel
 * BridgeLeave
 * BridgeListItem
 * BridgeVideoSourceUpdate
 * CallAnswered
 * CallForward
 * Cdr
 * CEL
 * ChallengeResponseFailed
 * ChallengeSent
 * ChannelTalkingStart
 * ChannelTalkingStop
 * ChannelUpdate
 * ChanSpyStart
 * ChanSpyStop
 * ConfbridgeEnd
 * ConfbridgeJoin
 * ConfbridgeLeave
 * ConfbridgeList
 * ConfbridgeListRooms
 * ConfbridgeMute
 * ConfbridgeRecord
 * ConfbridgeStart
 * ConfbridgeStopRecord
 * ConfbridgeTalking
 * ConfbridgeUnmute
 * ContactList
 * ContactListComplete
 * ContactStatus
 * ContactStatusDetail
 * CoreShowChannel
 * DAHDIChannel
 * DAHDIShowChannels
 * DBGetResponse
 * DeviceStateChange
 * DeviceStateListComplete
 * DeviceStatus
 * Dial
 * DialBegin
 * DialEnd
 * DialState
 * DND
 * DNDState
 * DongleNewCUSD
 * DongleNewUSSD
 * DongleNewUSSDBase64
 * DongleSMSStatus
 * DongleStatus
 * DongleUSSDStatus
 * DTMF
 * DTMFBegin
 * DTMFEnd
 * EndpointDetail
 * EndpointDetailComplete
 * EndpointList
 * EventFactoryImpl.php
 * EventMessage.php
 * ExtensionStateListComplete
 * ExtensionStatus
 * Factory
 * FailedACL
 * FAXSession
 * FAXSessionsComplete
 * FAXSessionsEntry
 * FAXStats
 * FAXStatus
 * FullyBooted
 * Hangup
 * HangupHandlerPop
 * HangupHandlerPush
 * HangupHandlerRun
 * HangupRequest
 * Hold
 * IdentifyDetail
 * Impl
 * InboundRegistrationDetail
 * InvalidAccountID
 * InvalidPassword
 * InvalidTransport
 * JabberEvent
 * Join
 * Leave
 * Link
 * ListDialplan
 * Load
 * LoadAverageLimit
 * LocalBridge
 * LocalOptimizationBegin
 * LocalOptimizationEnd
 * Masquerade
 * MCID
 * MeetmeEnd
 * MeetmeJoin
 * MeetmeLeave
 * MeetmeMute
 * MeetmeTalking
 * MeetmeTalkRequest
 * MemoryLimit
 * MessageWaiting
 * MiniVoiceMail
 * MonitorStart
 * MonitorStop
 * MusicOnHold
 * MusicOnHoldStart
 * MusicOnHoldStop
 * MWIGet
 * MWIGetComplete
 * NewAccountCode
 * NewCallerid
 * Newchannel
 * NewConnectedLine
 * Newexten
 * Newstate
 * OriginateResponse
 * OutboundRegistrationDetail
 * OutboundSubscriptionDetail
 * ParkedCall
 * ParkedCallGiveUp
 * ParkedCallSwap
 * ParkedCallTimeOut
 * PeerStatus
 * Pickup
 * PresenceStateChange
 * PresenceStateListComplete
 * PresenceStatus
 * QueueCallerAbandon
 * QueueCallerJoin
 * QueueCallerLeave
 * QueueMember
 * QueueMemberAdded
 * QueueMemberPause
 * QueueMemberPaused
 * QueueMemberPenalty
 * QueueMemberRemoved
 * QueueMemberRinginuse
 * QueueMemberStatus
 * QueueParams
 * QueueSummary
 * ReceiveFAX
 * Registry
 * Reload
 * Rename
 * RequestBadFormat
 * RequestNotAllowed
 * RequestNotSupported
 * ResourceListDetail
 * RTCPReceived
 * RTCPReceiverStat
 * RTCPSent
 * RTPReceiverStat
 * RTPSenderStat
 * SCCPConfEnd
 * SCCPConfEntered
 * SCCPConfLeave
 * SCCPConfLeft
 * SCCPConfLock
 * SCCPConfParticipantKicked
 * SCCPConfParticipantMute
 * SCCPConfParticipantPromotion
 * SCCPConfStart
 * SCCPConfStarted
 * SCCPShowDevice
 * SCCPShowLine
 * SendFAX
 * SessionLimit
 * SessionTimeout
 * Shutdown
 * SIPQualifyPeerDone
 * SoftHangupRequest
 * SpanAlarm
 * SpanAlarmClear
 * Status
 * Success
 * SuccessfulAuth
 * TableEnd
 * TableStart
 * Transfer
 * TransportDetail
 * UnexpectedAddress
 * Unhold
 * Unknown
 * Unlink
 * Unload
 * UnParkedCall
 * UserEvent
 * VarSet
 * VgsmMeState
 * VgsmNetState
 * VgsmSmsRx

# Currently Supported Actions

 * AbsoluteTimeout
 * Action
 * ActionMessage.php
 * AgentLogoff
 * Agents
 * AGI
 * AttendedTransfer
 * Atxfer
 * BlindTransfer
 * Bridge
 * BridgeDestroy
 * BridgeInfo
 * BridgeKick
 * BridgeList
 * BridgeTechnologyList
 * BridgeTechnologySuspend
 * BridgeTechnologyUnsuspend
 * CancelAtxfer
 * Challenge
 * ChangeMonitor
 * Command
 * ConfbridgeKick
 * ConfbridgeList
 * ConfbridgeListRooms
 * ConfbridgeLock
 * ConfbridgeMute
 * ConfbridgeSetSingleVideoSrc
 * ConfbridgeStartRecord
 * ConfbridgeStopRecord
 * ConfbridgeUnlock
 * ConfbridgeUnmute
 * ControlPlayback
 * CoreSettings
 * CoreShowChannels
 * CoreStatus
 * CreateConfig
 * DAHDIDialOffHook
 * DAHDIDNDOff
 * DAHDIDNDOn
 * DAHDIHangup
 * DAHDIRestart
 * DAHDIShowChannels
 * DAHDITransfer
 * DBDel
 * DBDelTree
 * DBGet
 * DBPut
 * DeviceStateList
 * DialplanExtensionAdd
 * DialplanExtensionRemove
 * DongleReload
 * DongleReset
 * DongleRestart
 * DongleSendPDU
 * DongleSendSMS
 * DongleSendUSSD
 * DongleShowDevices
 * DongleStart
 * DongleStop
 * Events
 * ExtensionState
 * ExtensionStateList
 * FAXSession
 * FAXSessions
 * FAXStats
 * Filter
 * GetConfig
 * GetConfigJSON
 * GetVar
 * Hangup
 * IAXnetstats
 * IAXpeerlist
 * IAXpeers
 * IAXregistry
 * JabberSend
 * ListCategories
 * ListCommands
 * LocalOptimizeAway
 * LoggerRotate
 * Login
 * Logoff
 * MailboxCount
 * MailboxStatus
 * MeetmeList
 * MeetmeListRooms
 * MeetmeMute
 * MeetmeUnmute
 * MixMonitor
 * MixMonitorMute
 * ModuleCheck
 * ModuleLoad
 * ModuleReload
 * ModuleUnload
 * Monitor
 * MuteAudio
 * MWIDelete
 * MWIGet
 * MWIUpdate
 * Originate
 * Park
 * ParkedCalls
 * Parkinglots
 * PauseMonitor
 * Ping
 * PJSIPNotify
 * PJSIPQualify
 * PJSIPRegister
 * PJSIPShowAors
 * PJSIPShowAuths
 * PJSIPShowContacts
 * PJSIPShowEndpoint
 * PJSIPShowEndpoints
 * PJSIPShowRegistrationInboundContactStatuses
 * PJSIPShowRegistrationsInbound
 * PJSIPShowRegistrationsOutbound
 * PJSIPShowResourceLists
 * PJSIPShowSubscriptionsInbound
 * PJSIPShowSubscriptionsOutbound
 * PJSIPUnregister
 * PlayDTMF
 * PresenceState
 * PresenceStateList
 * PRIDebugFileSet
 * PRIDebugFileUnset
 * PRIDebugSet
 * PRIShowSpans
 * QueueAdd
 * QueueChangePriorityCaller
 * QueueLog
 * QueueMemberRingInUse
 * QueueMemberRingInUse.php
 * QueuePause
 * QueuePenalty
 * QueueReload
 * QueueRemove
 * QueueReset
 * QueueRule
 * Queues
 * QueueStatus
 * QueueSummary
 * QueueUnpause
 * Redirect
 * Reload
 * SCCPAnswerCall
 * SCCPConference
 * SCCPConfigMetaData
 * SCCPDeviceAddLine
 * SCCPDeviceRestart
 * SCCPDeviceSetDND
 * SCCPDeviceUpdate
 * SCCPDndDevice
 * SCCPHangupCall
 * SCCPHoldCall
 * SCCPLineForwardUpdate
 * SCCPMessageDevice
 * SCCPMessageDevices
 * SCCPMicrophone
 * SCCPShowChannels
 * SCCPShowConference
 * SCCPShowConferences
 * SCCPShowDevice
 * SCCPShowDevices
 * SCCPShowGlobals
 * SCCPShowHintLineStates
 * SCCPShowHintSubscriptions
 * SCCPShowLine
 * SCCPShowLines
 * SCCPShowMWISubscriptions
 * SCCPShowSessions
 * SCCPShowSoftkeySets
 * SCCPStartCall
 * SCCPSystemMessage
 * SCCPTokenAck
 * SendText
 * SetVar
 * ShowDialPlan
 * SIPNotify
 * SIPPeers
 * SIPpeerstatus
 * SIPQualifyPeer
 * SIPShowPeer
 * SIPShowRegistry
 * SKINNYdevices
 * SKINNYlines
 * SKINNYshowdevice
 * SKINNYshowline
 * SorceryMemoryCacheExpire
 * SorceryMemoryCacheExpireObject
 * SorceryMemoryCachePopulate
 * SorceryMemoryCacheStale
 * SorceryMemoryCacheStaleObject
 * Status
 * StopMixMonitor
 * StopMonitor
 * UnpauseMonitor
 * UpdateConfig
 * UserEvent
 * VGSMSMSTx
 * VoicemailRefresh
 * VoicemailUsersList
 * VoicemailUserStatus
 * WaitEvent

## Debugging, logging

You can optionally set a [PSR-3](http://www.php-fig.org/psr/psr-3/) compatible logger:
```php
$pami->setLogger($logger);
```

By default, the client will use the [NullLogger](http://www.php-fig.org/psr/psr-3/#1-4-helper-classes-and-interfaces).

# Developers
This project uses [phing](https://www.phing.info/). Current tasks include:
 * test: Runs [PHPUnit](https://phpunit.de/).
 * cs: Runs [CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer).
 * doc: Runs [PhpDocumentor](http://www.phpdoc.org/).
 * md: runs [PHPMD](http://phpmd.org/).
 * build: This is the default task, and will run all the other tasks.

## Running a phing task
To run a task, just do:

```sh
vendor/bin/phing build
```

## Contributing
To contribute:
 * Make sure you open a **concise** and **short** pull request.
 * Throw in any needed unit tests to accomodate the new code or the
 changes involved.
 * Run `phing` and make sure everything is ok before submitting the pull
 request (make phpmd and CodeSniffer happy, also make sure that phpDocumentor
 does not throw any warnings, since all our documentation is automatically
 generated).
 * Your code must comply with [PSR-2](http://www.php-fig.org/psr/psr-2/),
 CodeSniffer should take care of that.

LICENSE
=======
Copyright 2016 Marcelo Gornstein <marcelog@gmail.com>

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

# Maintainer of This Fork:

* Diederik de Groot <ddegroot@talon.nl>

* Added the SCCP Message Implementation.
* ResponseHandling for ReponseEvents returned from ActionMessages that
  return multiple/complex results [Orig PR:73](https://github.com/marcelog/PAMI/pull/73).
* Note: Had some help from Jacob Kiers on the ResponseHandling.

# Thanks To:

* First a formost Marcelo Gornstein, the original designer and developer of
this module. See:[https://github.com/marcelog/PAMI](https://github.com/marcelog/PAMI)
for more information

* Jason Blank <rumpled at github> for helping in the debugging of the queue
functionality and some other ami inconsistencies.

* Francesco Usseglio Gaudi, for help in debugging the Originate action.

* Matías Barletta, for the vgms support.

* Eli Hunter, for helping in bringing in tls compatibility.

* Freddy dafredmail at googlemail, for his help and testing environment to add
dongle support.

* Joshua Elson for his help in trying and debugging in loaded asterisk servers.

* Jacob Kiers for his help in bringing in and testing async agi functionality,
and CEL event support.

* Richard Baar for noticing the lack of eof support when reading from socket,
the JabberEvent, and the ScreenName in JabberAction.

* Scot Opell for helping in debugging stream_get_line() in 5.3.9 and 5.3.10.

* Brian (wormling) for trying and fixing bugs on asyncagi.

* Henning Bragge for helping with newstate event and queues.

* mbonneau for ParkedCall and UnParkedCall events.

* @brenard : Updates to ConfBridge. See:[Orig PR:179](https://github.com/marcelog/PAMI/pull/179)

* @NikolayRevin: Add action QueueMemberRingInUse and updated: QueueMemberEvent and
QueueParamsEvent. See:[Orig PR:177](https://github.com/marcelog/PAMI/pull/177).

* @alexmnv: Added `getSocket()` method to `ClientImpl` class. See:[Orig
  PR:169](https://github.com/marcelog/PAMI/pull/169).

* @edigomes: Added Options-XXXXXX. See:[Orig PR:162](https://github.com/marcelog/PAMI/pull/162).

* @amir200xven: Added CDR EVent. See:[Orig PR:159](https://github.com/marcelog/PAMI/pull/159).

* @wizzle: Added PJSIPShowEndpoints et al. See:[Orig PR:158](https://github.com/marcelog/PAMI/pull/158)
and [Orig PR:157](https://github.com/marcelog/PAMI/pull/157).

* @syco: Extended MonitorAction. See:[Orig PR:149](https://github.com/marcelog/PAMI/pull/149).

* @ilgiz-badamshin: Extended AsyncAgi impl. See:[Orig
  PR:143](https://github.com/marcelog/PAMI/pull/143).

* @Adrian0350: Added DAHDIChannelEvent. See:[Orig
  PR:138](https://github.com/marcelog/PAMI/pull/138).

* @Bloodoff: Added DeviceStateChange/VarSet. See:[Orig
  PR:126](https://github.com/marcelog/PAMI/pull/126).

* @thomasvargiu: Fix getMessages. See:[Orig PR:122](https://github.com/marcelog/PAMI/pull/122).

* @sctt: Added event filters. See:[Orig PR:107](https://github.com/marcelog/PAMI/pull/107).

* @alesf: Added QueueEntry. See:[Orig PR:98](https://github.com/marcelog/PAMI/pull/98).

* @parhamdoustdar: ConfBridge et al. See:[Orig PR:80](https://github.com/marcelog/PAMI/pull/80).
