# Schlüsselwörter

## Regeln

Eine Regel ist das wichtigste Konstrukt in der Grammatik von openVALIDATION. Diese bestehen aus einer Bedingung und einer Aktion. Bei einer Validierungsregel ist die Aktion immer eine Fehlermeldung. Die einfachste Möglichkeit so eine Regel auszudrücken ist ein **WENN** / **DANN** Konstrukt.

| Schlüsselwort | Beschreibung |
| :--- | :--- |
| `WENN, SOLLTE` | Markiert den Anfang einer Regel und der darauffolgenden Bedingung |
| `DANN` | Markiert den Beginn einer Fehlermeldung |
| `UND` | Markiert den Beginn einer neuen verknüpften UND Bedingung |
| `ODER` | Markiert den Beginn einer neuen verknüpften ODER Bedingung |

## Implizite Bedingungen / Alternativer Regelausdruck

<table>
  <thead>
    <tr>
      <th style="text-align:left">Schl&#xFC;sselwort</th>
      <th style="text-align:left">Beschreibung</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>MUSS, M&#xDC;SSEN, SOLL, SOLLEN, DARF, D&#xDC;RFEN</code>
      </td>
      <td style="text-align:left">
        <p>Ein Indikator identifiziert einen Ausdruck, als Regel.</p>
        <p><em>Die Bedingung in so einem MUSS Ausdruck enth&#xE4;lt eine Implizite Negation!</em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>MUSS NICHT, M&#xDC;SSEN NICHT, SOLL NICHT, SOLLEN NICHT, DARF NICHT, D&#xDC;RFEN NICHT</code>
      </td>
      <td style="text-align:left">Ein Indikator identifiziert einen Ausdruck, als Regel.</td>
    </tr>
  </tbody>
</table>## Vergleichsoperatoren

Eine Vergleichsoperation hat immer einen linken und einen rechten Operanden und den entsprechenden Vergleichsoperator. 

| Schlüsselwort | Beschreibung |
| :--- | :--- |
| `GLEICH, IST` | Ein Vergleichsoperator '=' für numerische und String Operanden |
| `UNGLEICH, NICHT, KEIN` | Ein Vergleichsoperator '!=' für numerische und String Operanden |
| `KLEINER, NIEDRIGER , KÜRZER, WENIGER, GERINGER` | Ein Vergleichsoperator '&lt;' für numerische Operanden |
| `GRÖßER, MEHR, HÖHER, LÄNGER, ÜBERSTEIGT, ÜBERSTEIGEN` | Ein Vergleichsoperator '&gt;' für numerische Operanden |
| `GRÖßER ODER GLEICH, MEHR ODER GLEICH, MINDESTENS` | Ein Vergleichsoperator '&gt;=' für numerische Operanden |
| `KLEINER ODER GLEICH, WENIGER ALS ODER GLEICH, MAXIMAL` | Ein Vergleichsoperator '&lt;=' für numerische Operanden |
| `VORHANDEN, EXISTIERT, GEGEBEN, ANGEGEBEN, GIBT` | Ein Vergleichsoperator für "nicht null" |
| `NICHT VORHANDEN, NICHT EXISTIERT, NICHT GEGEBEN, NICHT ANGEGEBEN, NICHT GIBT` | Ein Vergleichsoperator für "null" |

## Arithmetik

| Schlüsselwort | Beschreibung |
| :--- | :--- |
| `+, PLUS` | Eine mathematische Operation für eine einfache Addition |
| `-, MINUS` | Eine mathematische Operation für eine einfache Subtraktion |
| `*, MAL` | Eine mathematische Operation für eine einfache Multiplikation |
| `/, DURCH, GETEILT DURCH` | Eine mathematische Operation für eine einfache Division |
| `MOD, MODULO` | Eine mathematische Operation für eine einfache Modulo - Berechnung |

## Kommentar

Man kann in dem Regelwerk auch Kommentare verfassen. Diese enthalten keinerlei Logik.

| Schlüsselwort | Beschreibung |
| :--- | :--- |
| `KOMMENTAR` | Ein Kommentar |

## Fehlermeldung

| Schlüsselwort | Beschreibung |
| :--- | :--- |
| `ERRORCODE, MIT ERRORCODE, MIT CODE` | Fehlermeldungen enthalten oft eigene Fehlercodes zur eindeutigen Identifikation.  |

