Requirements:

-Knowledge of Resistance: http://en.wikipedia.org/wiki/The_Resistance_(game)
-Protege, and knowledge of either RDFS, OWL, or Protege usage.
-Any other reasoner deployments capable of loading owl files.

Intructions:

Load ResistanceOntology.owk into protege and run the Hermit reasoner.
This ontology comes with example instance data including a real life Resistance Match!
See the Usage section below.

Ontology Terms Explanation:

Player - Represents players in the game.
Resistance - A resistance player.  Never lies, can only cast success votes.
VouchedResistance - A player who was vouched (called resistance during player check card) by a proven resistance player.
Spy - A spy
PlayerLyingSpy - A player who has lied about other players at some point (as computed by reasoning)
VoteLyingSpy - A player who has lied about a vote at some point (as computed by reasoning)
Vote - Votes cast by players, only spies can cast fails.
SpyVote - A vote belonging to a spy.
FailVote - A vote failing the mission.
AccusedVote - A vote accused of being a fail by a proven resistance member.
ResistanceVote - A vote cast by a proven resistance member, must be a success.
VouchedVote - A vote that was vouched to be success by a proven resistance member.

RDFS subset relations:

Thing
	Player
		Resistance
			VouchedResistance
		Spy
			AccusedSpy
			PlayerLyingSpy
			VoteLyingSpy
	Vote
		SpyVote
			FailVote
				AccusedVote
		SuccessVote
			ResistanceVote
			VouchedVote

RDFS equivalence relations:

SpyVote = belongsTo some Spy
FailVote = hasVoteValue "fail"
AccusedVote = voteAccusedBy some Resistance
SuccessVote = hasVoteValue "success"
ResistanceVote = belongsTo some Resistance
VouchedVote = voteVouchedBy some Resistance

Usage:

Clear out all individual instances in the individual's tab.

Create a Resource for each player in the game.
Create a single Match resource.
	Add a hasMember exactly ${number of spies in game} property to the Match resource.
	Add a hasMember exactly ${number of resistance in game} property to the Match resource.
	Add a hasMember Only ${list of all players in the game} property to the Match resource.

For each voting event create a VotingEvent resource
	Add a containsVote exactly ${number of fail votes} (hasVoteValue value "fail") property to the VotingEvent property.
	Add a containsVote exactly ${number of success votes} (hasVoteValue value "success") to the VotingEvent property.
	Add a conainsVote only (hasVoteValue exactly 1 string) to the VotingEvent property.
	Add a Vote resource for each player who voted.
		Add a belongsTo ${player} property for each vote.
	Add a containsVote only (${ list of vote resources cast this round}) to the VotingEvent property.

For each mission/check cards there is an associated property you can assign to each player resource.

	AccusesPlayer is when one player checks another character's card and calls them a spy.
	VouchesPlayer is when one player checks another player's card and calls them resistance.
	AccusesVote is when one player checks another player's cast vote and calls it a fail vote.
	VouchesVote is when one player checks another player's cast vote and calls it a success vote.
