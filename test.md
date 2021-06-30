  ------------------------------------------------------------
  Install & Configure
  Automatic Key Rotation - OTG
  Systemdokumentasjon
  Skrevet av Anders Julton (anders\@nimtech.no) -- Juli 2021
  ------------------------------------------------------------

Innholdsfortegnelse

[1. Revisjon 3](#revisjon)

[2. Informasjon 4](#_Toc530343716)

[2.1 Bilde av komplett løsning: 4](#bilde-av-komplett-løsning)

[3. Systeminfo 5](#systeminfo)

[3.1 Lisenser 5](#_Toc530343719)

[3.2 Oversikt 5](#_Toc530343720)

[4. Installasjonsprosedyre 6](#_Toc530343721)

[4.1 Konfigurasjon av CA-server 6](#_Toc530343722)

[4.2 Installasjon NDES rollen 10](#_Toc530343723)

[4.3 Konfigurasjon NDES server 11](#_Toc530343724)

[4.4 Azure AD Application Proxy 23](#_Toc530343725)

[5. Endringslogg 26](#endringslogg)

# Revisjon

  Versjon   Revidert av     Dato       Kommentar
  --------- --------------- ---------- -----------------------------
  1.0       Anders Julton   30.06.21   Opprettelse, første versjon
                                       
                                       

# Informasjon

Automatic Key Rotation (AKR) sørger for automatisk og periodisk
oppdatering av keys for service connections i Azure DevOps.

AKR må installeres og konfigureres separat på hvert prosjekt i Azure
DevOps hvor løsningen skal implementeres.

## Bilde av komplett løsning:

![Diagram Description automatically
generated](media/image1.png){width="6.298611111111111in"
height="3.3805555555555555in"}

# Systeminfo

###  {#section .list-paragraph}

# Installasjonsprosedyre

## Konfigurer prosjekt i Azure DevOps

-   Under Project Settings -\> Pipeline Settings: "Limit job
    authorization scope to current project for non-release pipelines" må
    være aktivert.

-   Under Project Settings -\> Permissions: Legg til "Build
    Service"-brukeren til Endpoint Administrators. Navnet til denne
    brukeren starter med prosjektnavnet.

![Graphical user interface, application, Teams Description automatically
generated](media/image2.png){width="6.298611111111111in"
height="4.1930555555555555in"}

## Last opp og konfigurer nødvendige filer

### Informasjon

> Løsningen krever et sett med filer som lastet opp i et nytt repo på
> prosjektet. Filene klones fra Nimtech sin Github-profil. Filene må
> endres til å speile prosjektspesifikk informasjon

###  Prosedyre

1.  Importer følgende Github-repository:
    <https://github.com/nimling/AZD_AutoKeyRotation>

2.  I filen (prosjektrepo)\\pipelines\\key-rotation.yml

    a.  Endre verdien *serviceConnectionNameLUTS* til navnet på service
        connection som løsningen skal implementeres for.

        i.  Dersom prosjektet inneholder flere service connections må
            det for hver av disse opprettes variabler og kopi av linje
            24-30.

    b.  Under branches -\> include: Endre -mainer til -main (navnet på
        hovedgrenen i repoet)

## Endre rettigheter til app registration

### Informasjon

> App registration som brukes av service connection trenger spesifikke
> rettigheter: Den må eie seg selv, samt ha mulighet til å generere nye
> secrets/keys. Dette gjøres automatisk med et av scriptene i det nye
> prosjektrepoet.

### Prosedyre

1.  Last ned og kjør filen
    (prosjektrepo)\\scripts\\Set-addApplicationOwner.ps1 lokalt.

2.  I terminalvinduet:

    a.  Skriv inn gjeldende Tenant ID

    b.  Skriv inn Application ID for Application som tilhører service
        connections (ved flere connections i samme prosjekt, kjør
        Set-addApplicationOwner.ps1 for hver av disse, med de respektive
        applikasjons-IDene)

    c.  Åpne webside som er linket i terminalvinduet og logg inn med
        valideringskoden fra terminalvinduet.

3.  Etter programmet er kjørt, bekreft at applikasjonen er satt som eier
    av seg selv, samt har ReadWrite.OwnedBy under API permissions.

![Graphical user interface, text, application, email Description
automatically generated](media/image3.png){width="6.298611111111111in"
height="2.816666666666667in"}

![Graphical user interface, text, application Description automatically
generated](media/image4.png){width="6.298611111111111in"
height="3.0145833333333334in"}

## Sett opp pipeline

### Informasjon

> En pipeline settes opp for å daglig generere nye secrets og slette
> gamle. Det nye prosjektrepoet inneholder pipeline-filen, samt et
> tilhørende script. Scriptet genererer ny secret på app registration og
> på service connection, samt sletter gamle secrets den selv har
> generert. Eksisterende secrets som ikke er generert av scriptet må
> eventuelt slettes manuelt.

### Prosedyre

1.  Lag ny pipeline for prosjektet

2.  Velg Azure Repos Git

3.  Velg prosjektrepoet som inneholder de nylig opplastede filene.

4.  Velg «Existing Azure Pipelines YAML file" og velg filen
    key-rotation.yml

# Endringslogg

Alle endringer/feilrettinger på systemet skal loggføres her. Dette kan
være service packs, oppdateringer, endring i konfigurasjon osv.

  **Hva er endret/feilrettet**   **Dato**   **Utført av**
  ------------------------------ ---------- ---------------
                                            
                                            
                                            
                                            
                                            
                                            
                                            
                                            
                                            
