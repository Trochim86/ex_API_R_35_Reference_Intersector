# Reference Intersector

## Metadata
- **Plik źródłowy:** `Reference+Intersector+`
- **Typ:** C#
- **Kategoria:** Geometria i Raycasting
- **Numer:** 27

## Opis

Użycie ReferenceIntersector do znajdowania elementów przecinających promień w przestrzeni 3D.

## Główne komponenty i funkcje

### Metody

- `referenceIntersector()` - metoda do wykonania operacji

## Użyte klasy i interfejsy

### Klasy Revit API
- `Document`
- `Element`
- `Line`
- `Reference`
- `ReferenceIntersector`
- `TaskDialog`
- `UIDocument`
- `View`
- `XYZ`

### Klasy własne
Brak własnych klas.

### Implementowane interfejsy
Brak zaimplementowanych interfejsów.

## Namespaces

```csharp
// Brak using statements
```

## Parametry i wartości zwracane

Brak formalnych parametrów wejściowych/wyjściowych.

## Przypadki użycia

1. Automatyzacja powtarzalnych zadań w Revit
2. Zwiększenie efektywności pracy
3. Standaryzacja procesów modelowania

## Szczegóły implementacji

### Operacje geometryczne
Implementuje zaawansowane operacje na geometrii:
- Obliczenia wektorowe (XYZ)
- Tworzenie i manipulacja krzywymi
- Operacje na bryłach (Solid)

### Interakcja z użytkownikiem
Wykorzystuje elementy UI do komunikacji:
- `TaskDialog` dla komunikatów
- `PickObject` dla selekcji elementów
- Formularze Windows Forms dla złożonych input'ów


## Najlepsze praktyki

1. **Zarządzanie transakcjami**: Zawsze zamykaj transakcje w blokach `using`
2. **Obsługa błędów**: Używaj try-catch dla operacji mogących zakończyć się niepowodzeniem
3. **Walidacja input'u**: Sprawdzaj dane wprowadzone przez użytkownika
5. **Dokumentacja kodu**: Komentuj nieoczywiste logiki biznesowe
6. **Testowanie**: Testuj na różnych wersjach Revit i typach projektów

## Powiązane rozszerzenia i narzędzia

### Narzędzia deweloperskie
- **RevitLookup** - inspekcja elementów i ich właściwości
- **Revit Python Shell** - interaktywne testowanie API
- **pyRevit** - framework do rozwijania narzędzi w Pythonie

### Biblioteki pomocnicze
- **RevitAPI.dll** - główna biblioteka Revit API
- **RevitAPIUI.dll** - komponenty interfejsu użytkownika

## Uwagi i ostrzeżenia

⚠ **Wątkowość UI** - operacje UI muszą być wykonywane w głównym wątku Revit

## Kod źródłowy

```csharp
public void referenceIntersector()
		{
			Document doc = this.ActiveUIDocument.Document;
			View3D view3d = doc.ActiveView as View3D;
			if (view3d == null)
			{
				TaskDialog.Show("Error","Active view must be a 3d view");
				return;
			}
			ReferenceIntersector ri = new ReferenceIntersector(view3d);
			IList<ReferenceWithContext> refWithContextList = ri.Find(XYZ.Zero,XYZ.BasisY);
			string data = "";
			foreach (ReferenceWithContext rwc in refWithContextList)
			{
				Reference r = rwc.GetReference();
				double d = rwc.Proximity;
				Element e = doc.GetElement(r);
				
				GeometryObject o = e.GetGeometryObjectFromReference(r);
				string oType = o.GetType().ToString();
				data += oType + " - " + e.Name + " - " + d + Environment.NewLine;
			}
			TaskDialog.Show("elements hit",data);
		}
```

---

**Data aktualizacji:** 2025-11-12
**Wersja dokumentacji:** 1.0
