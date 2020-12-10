- take a look again at fast/strong memorization methods
	* a big problem with sm2 et al is that their use is a grueling job
		. regularity is one thing,
		but i think the biggest problem is actually adding cards
		. if we could automate card generation, make high-quality
		cards with as little effort as possible, it would be a lot
		easier to muster motivation to use it daily
		. chinese: hard because adding cards is super tedious
		. german, stats, etc: same, despite everything
		. medicine: exacerbated due to daily volume of new info
			- very hard to apply the method and use regularly
			due to the high effort of adding new shit
		. i think that's the biggest show stopper, if we have good
		decks and just have to go through them daily, it's alright
		⇒ how to automate deck generation?
			- decompose information to learn into types
			- simplest is sentences with holes in them
				* we could just take a text document and shit
				it to a tool, then actually use an interactive
				approach to select word(s) in the sentences to
				mask; the tool then cuts everything into
				sentences and generates cards for each with
				masked passages
				* multiple cards per sentence → just increase
				a mask counter or something
				* select nothing → discard sentence
				* this takes care of the most basic learning by heart
				large volumes of text data
			- vocabulary/explaining a concept
				* provide a huge dictionary of term:definition and
				generate cards for each entry
				* again, for learning shit by heart,
				can still use the body of text idea,
				by just masking an explanation,
				instead of making a dictionary
				* problem now is getting these huge bodies of text
				* in medschool, the texts are provided
				* for languages, they have to be mined
				* either find textual dictionaries with convenient formats
				* or crawl a site, taking first definition etc
			- chinese: meaning/stroke order/radicals/pronounciation(s)
				* more complex and has to be decomposed into multiple
				cards per ideograph
				* again, problem is to find a database to work with
			- more complex: interpret image
				* provide an image and a question/answer for it
				→ create a single card with the image and the question
				* still some effort
			- the more complex the question or cognitive process,
			the harder it will be to generate a card
			- so, find other primitive tasks to enrich the deck
			automatically with
	* the mindmap actually has the same problem
		. adding new sections, linking stuff, is tedious
		. so we end up overloading one or more pages pending something better
		. scripts to add sentences/paragraphs to a given note/section?
		. scripts to link stuff together? scripts to insert links?
		. still haven't done conversion to html/pdf/whatever,
		or the site idea
	* deck generation is one thing, next is a better algorithm
		. maybe go on arxiv and search for 'supermemo',
		get other published methods, evaluate which are worth trying
	* a better algorithm is one thing, next is a different approach
		. ie. other than spaced repetition,
		but how to find out about that?
	* this all is for long term memorization,
	but what if we're a lazy ass like me,
	and need a method to memorize stuff fast short term?
		. ie. master's degree, super shitty subject we don't want to spend time on,
		and we don't really care about for later
		. ie. many if not most subjects
		. zB genetics: stupid and boring and yet huge
		. we can't apply spaced repetition algos here,
		because we want minimal effort,
		and minimal time
			- we do it a few days before an exam
			- we can't use an algo that spreads stuff out over weeks
		. might end up being a simpler task indeed BECAUSE we don't need spaced repetition
		. ie. just simple repetition, but using the same decks
		. no card pruning, just look over everything
		. on the other hand, this naive approach is equivalent
		(and worse wrt effort)
		to just reading everything once per day
		. genetics → >150 pages of bullshit,
		if we could read them daily, we wouldn't need this shit
		. so, there must be a better way
		. in real life, what we do is essentially go through everything,
		mark sections deemed important,
		discard everything else,
		and maybe reread important sections,
		perhaps even multiple times,
		just prior to the exam
		. so, we could maybe use an interactive approach,
		where we mark sections of text as important
			- or assign a score 1-5 rather than true/false
		. then, we essentially generate cards with each section
		. then, we go through stuff,
		lowering importance when shit is deemed understood and memorized
		. card is just the information, no question/answer, not an actual flashcard
		. data types: bodies of text, slides, handwritten notes, exercises
			- slides: group them together in sections and do the same
			- notes: no ocr, just scan and crop to sections
		. exercises: no choice? there must be an easier way
			- resolve them → one card, one exercise
			- importance: difficulty and/or frequency on exam
			- if we find time and motivation,
			this ranks exercises to redo by importance
	* a super common pattern of memorization techniques
	is storytelling
		. decompose information to memorize as stories
		with individual quanta of information as parts of the story
		. this works very well, but for a specific type of task
			* can it be generalized?
		. but is it applicable to huge volumes of information?
			* generate and learn a story for each paragraph
			in a huge volume of medschool texts?
			* generate long stories?
			* now you have to learn stories on top of learning
			the actual information?
		. it is necessary to be able to generate stories well and quickly
			* i can't
			* it's a skill and a reflex one must acquire
		. it's worth it in general, but it would take a lot practice
		. not sure there are tools that can speed up the process
		. and dunno if this is ok for short term or long term or both
	* either way, important to find existing work
		. arxiv, but possibly elsewhere
		. pubmed for physiopathological point of view?
		. blogposts from internet rando nerds, but low quality
		. coursera: learning how to learn course
		. ironbrain: blog post on sudonull and code on github, might be interesting
			* same guy: kciray8/IceMemo: anki applied to speech in movies
	* finally, all this has to be weighted wrt implementation of the necessary tools
		. there exists an appropriate tool → great
		. but usually there won't be one
		. stuff that can be reduced to shell scripts: relatively fast
		. interactive/graphical shit → fuck
		. time design + time implementation + time testing/incremental improvements
		. compounded if evaluating multiple methods
		. also, total time developing framework for a method assumes the method
		is worth it
			- if we don't know before hand which method is optimal in general,
			and which is optimal for ourselves,
			we're up shits creek without a paddle if we choose wrong
			- it would take time to find out if the method is usable at all
				* on the other hand, one could argue that time spent studying
				is always useful regardless of method,
				just with different gain/effort ratios
			- can't really know if we can do better if we can't test multiple methods
			- can't really improve handing of individual method if no time to try stuff out
	* efficient memorization is one thing,
	but there's other factors involved in learning
		. specific efficient methods for specific tasks
		. "cognicists" out there teach such methods (youtube, books, online courses?)
		. mental hygene plays a big role
			* lots of little things contribute to overall performance
			* sleep, huge
			* food
			* added brain foods
			* fen-shui equivalent of keeping clean rooms, etc
			* meditation/etc actually important for inducing certain states
			* regularity, down to extremely repetitive daily schedule
			* discipline, obviously
		. rewiring brain pleasure circuits and cognitive behavioral therapy
			* we have harmful behavior acquired over time
				- pleasure seeking impeding on performance, etc
				- acquired risk/effort vs reward reflexes
				- acquired habits:
					* learned to watch tv, learned to spend time on social networks
					* equivalent to learned to spend attention in a specific way
					* experts say learning to spend time on twitter and al when getting up
					== learning to pay attention to a bunch of things simultaneously
					and thus be very distractable,
					AND doing this during a specific brain state after waking up,
					shaping other parts of subsequent behavior and learning
				- pathology-induced behaviors
					* in our case, 10 years of progressively worsening depression
					* resulted in a bunch of harmful behaviors
						- life sucks, so focus on whatever pleasures are left
						⇒ learned to spend no effort and only seek easy pleasure
						- lots of pleasure → low threshold,
						so learned to need a lot of it daily
						- life is pointless, therefore why spend time on anything
						⇒ initiating effort on anything is difficult
						- all this: low discipline, low focus, bad mental hygene
						- learning new information is among the hardest tasks,
						so now we just can't do it at all without huge effort
			* acquired behavior → have adapted to a certain lifestyle
			* but lifestyle intellectually unsatisfactory → have to change it
			* change → effort, with exponential complexity
				- the bigger then change
				- the bigger the implications
				- difficulty explodes
			* on the other hand, once a habit is made, the effort implied lowers more and more
				- hate sports, never do any → doing one exercise one day per month is super hard
				- decided to do biking every day → now have the motivation,
				but body is weak, and it's super hard
				- months later:
					* body strenghtened proportionally to effort spent
					* less and less intellectual effort involved in the task
					* now can do cycling for much longer without even thinking about it
					* in fact, have developped a taste or even need for it
			* cbt shows a good way, or at least one way, of altering behavior
			* taking the example of cycling, there's a long-term process to it
				- intiating it from sheer motivation
				- making it as regular practice
				- learning the behavior as in acquiring the habit
				- over time, difficulty necessarily decreases
				to the point of it becoming natural
			* this is applicable to sport just as it is applicable to lifestyle
			or any cognitive process,
			down to even worldviews and apprehending new information, people etc
			* there's a method to it, obviously
				- introduction of primitive low effort tasks as a basis for new behavior
				- gradually increasing regularity
				- and gradually increasing task intensity and difficulty
				- regular self evaluation, motivation, rewards
			* a body of research shows that the more a task is perceived as a game,
			the easier it is for the brain to be engaged in it
				- shown for schoolwork and any other shit
			* therefore, the more a task is made a game,
			the easier it is to work on it
				- our daily challenges idea is basically that,
				assigning score system to daily tasks,
				evaluation at end of day,
				and satisfaction/reward on accomplishments
				- also githubifying work: more commits, more points, more satisfaction
			* the more the gains are perceptible,
			the more it feels rewarding and motivates further effort
			* so, it makes sense to focus mostly on high gain tasks,
			and leaving the rest for later,
			when effort is minimized
	* all in all, worth exploring?
		. several ways of looking at it
			- either you consider that your studies are short,
			and at this point it's too late bothering with it
			- or your studies are longer,
			and it might be worth it by the end
			- or regardless of the situation,
			you either don't care enough,
			or don't want to expend such effort
			- or you consider that this is applicable for anything in life,
			over entire lifetime
		. definitely the latter
		. "soit on se dérange, soit on méprise"
			- no effort, no gain
			- and this has proven and huge gains
		. whether it is learning better behavior,
		or learning to learn better,
		this is extremely important for weirdo turbonerds like us,
		who only live for advancement, improvement and new knowledge and skills
		. so, might not be useful for this year's course since it's almost done,
		but it's useful for EVERYTHING ELSE
	* so, shut up and work, and just fucking do it already
		. daily routine of doing useful shit
		. and daily routine of learning new behavior
		. identify problem → find solution → add it to the behavioral todo list
- found references: ~/tr/learning on linux; anki notes on sm2 and alteration
