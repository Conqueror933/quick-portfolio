The Quizes main loop:<br/>
```c++
while (true)
	{
		int choice;
		choice = SafeInput::GetInt("Hauptmenu:\n"
			"01: Quiz starten\n"
			"02: Statistiken abrufen\n"
			"03: Pro Frage Statistiken abrufen\n"
			"04: Chance fuer falsch beantwortete Fragen anpassen\n"
			"05: Anzahl der Fragen pro Runde setzen\n"
			"06: Waehle andere Quizdatei aus\n"
			"07: Neue Frage hinzufuegen\n"
			"08: Art des Quizes aendern\n"
			"09: Zwischenspeichern\n"
			"00: Speichern und Beenden\n"
			"10: Beenden ohne Speichern\n"
			"99: Statistiken zuruecksetzen");

		switch (choice)
		{
		case 0:	//Beenden
			quiz->Safe();
		case 10: //NoSafe Quit
			return 0;
		case 1:	//Quiz starten
			quiz->Start(rng);
			break;
		case 2:	//Statistiken abrufen
			quiz->GetStatistics();
			break;
		case 3:	//Pro Frage Statistiken abrufen
			quiz->GetPQStatistics();
			break;
		case 4:	//Chance für falsch beantwortete Fragen anpassen
			quiz->SetChance(SafeInput::GetInt("Wie hoch soll die Chance fuer zuvor falsch beantwortete Fragen sein? (in %): "));
			break;
		case 5:	//Anzahl der Fragen pro Runde setzen
			quiz->SetRounds(SafeInput::GetInt("Wie viele Runden moechten Sie spielen? "));
			break;
		case 6:	//Wähle andere Quizdatei aus
			quiz->SetPath(SafeInput::GetPath("Geben Sie den vollständigen Pfad an. "));
			break;
		case 7:	//Neue Frage hinzufügen
			quiz->AddQuestion();
			break;
		case 8:	//Art des Quizes ändern
			if (quiztype == "1")
			{
				quiz = std::make_unique<MultipleChoiceQuiz>(SafeInput::GetPath("Pfad setzen: ", 1));
				quiztype = "2";
			}
			else
			{
				quiz = std::make_unique<InputBasedQuiz>(SafeInput::GetPath("Pfad setzen: ", 1));
				quiztype = "1";
			}
			break;
		case 9:
			quiz->Safe();
		case 99: //HardReset
			quiz->HardReset();
			break;
		default://wrong input
			//happens when a number is entered that is not assigned, like 23242525
			std::cout << "Wrong input.\n\n";
			break;
		}
	}
```
