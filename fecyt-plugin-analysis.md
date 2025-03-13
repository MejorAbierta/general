### Table of Contents
- [About Content](#about)
- [Article Documentation](#articles)
- [Author Information](#authors)
- [Reviewer Information](#reviewers)
- [Issues Documentation](#issues)
- [Journal Identification Information](#journal-id)
- [Submissions Page Information](#submissions-info)
- [URLs](#urls)
- [Editorial Flow Export](#editorial)


### About

```php
$withoutTags = strip_tags($context->getData('about', \AppLocale::getLocale()));

```

---
###  Articles
- Submited
- Accepted
- Rejected
- Published

```php
$data = "Nº de artículos para la revista ".Application::getContextDAO()->getById($this->contextId)->getPath();
$data .= " desde el ".date('d-m-Y', strtotime($dateFrom))." hasta el ".date('d-m-Y', strtotime($dateTo))."\n";
$data .= "Recibidos: ".$this->countSubmissionsReceived($submissionDao, [$this->contextId, $dateFrom, $dateTo])."\n";
$data .= "Aceptados: ".$this->countSubmissionsAccepted($submissionDao, [$this->contextId, $dateFrom, $dateTo])."\n";
$data .= "Rechazados: ".$this->countSubmissionsDeclined($submissionDao, [$this->contextId, $dateFrom, $dateTo])."\n";
$data .= "Publicados: ".$this->countSubmissionsPublished($submissionDao,[$this->contextId, $dateFrom, $dateTo]);
```
- Count submissions
```sql
SELECT COUNT(DISTINCT s.submission_id) AS count
	        FROM submissions s
            LEFT JOIN publications p on p.publication_id = (
                SELECT p2.publication_id
                FROM publications as p2
                WHERE p2.submission_id = s.submission_id
                  AND p2.status = ".STATUS_PUBLISHED."
                ORDER BY p2.date_published ASC
                LIMIT 1)
            WHERE s.context_id = ?
              AND s.submission_progress = 0
              AND (p.date_published IS NULL OR s.date_submitted < p.date_published)
              AND s.date_submitted >= ?
              AND s.date_submitted <= ?
```

- Accepted
```sql
SELECT COUNT(DISTINCT s.submission_id) AS count
            FROM submissions as s
            LEFT JOIN publications p on p.publication_id = (
                SELECT p2.publication_id
                FROM publications as p2
                WHERE p2.submission_id = s.submission_id
                  AND p2.status = ".STATUS_PUBLISHED."
                ORDER BY p2.date_published ASC
                LIMIT 1)
            LEFT JOIN edit_decisions as ed on s.submission_id = ed.submission_id
            WHERE s.context_id = ?
              AND s.submission_progress = 0
              AND (p.date_published IS NULL OR s.date_submitted < p.date_published)
              AND s.status != ".STATUS_DECLINED."
              AND ed.decision = 1
              AND ed.date_decided >= ?
              AND ed.date_decided <= ?
```

---
### Submissions

Get all submissions between 2 dates:

```php
   public function getSubmissions($params){
        $submissionDao = \DAORegistry::getDAO('SubmissionDAO'); /* @var $submissionDao \SubmissionDAO */

        return $submissionDao->retrieve("
```
```sql
			SELECT DISTINCT s.submission_id as id
	        FROM submissions s
            LEFT JOIN publications p on p.publication_id = (
                SELECT p2.publication_id
                FROM publications as p2
                WHERE p2.submission_id = s.submission_id
                  AND p2.status = ".STATUS_PUBLISHED."
                ORDER BY p2.date_published ASC
                LIMIT 1)
            WHERE s.context_id = ?
              AND s.submission_progress = 0
              AND (p.date_published IS NULL OR s.date_submitted < p.date_published)
              AND s.date_submitted >= ?
              AND s.date_submitted <= ?
            GROUP BY s.submission_id
```
```php
						", $params
            )->GetRows();
    }
```
---
Submissions rejected:
```php
    public function getSubmissionsDeclined($submissionDao, $params)
    {
        return $submissionDao->retrieve(
            "
```
```sql
            SELECT DISTINCT s.submission_id as id, ed.date_decided as date, s.current_publication_id as pub
            FROM submissions as s
            LEFT JOIN publications p on p.publication_id = (
                SELECT p2.publication_id
                FROM publications as p2
                WHERE p2.submission_id = s.submission_id
                  AND p2.status = ".STATUS_PUBLISHED."
                ORDER BY p2.date_published ASC
                LIMIT 1)
            LEFT JOIN edit_decisions as ed on s.submission_id = ed.submission_id
            WHERE s.context_id = ?
              AND s.submission_progress = 0
              AND (p.date_published IS NULL OR s.date_submitted < p.date_published)
              AND s.status = ".STATUS_DECLINED."
              AND ed.decision IN(4,9)
              AND ed.date_decided >= ?
              AND ed.date_decided <= ?
            GROUP BY s.submission_id, ed.date_decided, s.current_publication_id
```
```php
", $params
        )->GetRows();
    }
```






### Authors

```php
$submissionDao->getById($submissionItem['id']);
$authors = $submission->getAuthors();
```
Data collected per user:
- First name
- Last name
- Affiliation
- Email

```php
$author->getData('givenName');
$author->getData('familyName');
$author->getData('affiliation');
$author->getData('email');
```

### Reviewers

Get reviewers:

```php
function getReviewers($params)
    {
        $userDao = \DAORegistry::getDAO('UserDAO'); /* @var $userDao \UserDAO */

        return $userDao->retrieve(
            "
```
```sql
				SELECT
                    u.user_id as id,
                    u.username,
                    max(giv.setting_value) givenName,
                    max(fam.setting_value) familyName,
                    max(aff.setting_value) affiliation,
                    u.email
                FROM
                    users u
                        left outer join user_settings giv on (u.user_id = giv.user_id and giv.setting_name = 'givenName')
                        left outer join user_settings fam on (u.user_id = fam.user_id and fam.setting_name = 'familyName')
                        left outer join user_settings aff on (u.user_id = aff.user_id and aff.setting_name = 'affiliation')
                        left join user_user_groups grp on (u.user_id = grp.user_id)
                        left join user_group_settings ugs on (grp.user_group_id = ugs.user_group_id)
                        left join review_assignments ra ON (u.user_id = ra.reviewer_id)
                        left join submissions s ON (s.submission_id = ra.submission_id)
                WHERE
                    ugs.setting_value LIKE 'Revisor%'
                    AND ra.date_completed >= ?
                    AND ra.date_completed <= ?
                    AND s.context_id = ?
                GROUP BY
                    u.user_id,
                    u.username,
                    u.email;
```
```php
", $params
            )->GetRows();
    }
```
This line no longer works in 3.4, it doesn't find "UserDAO"
```php
 $userDao = \DAORegistry::getDAO('UserDAO');
```



---
### Issues

Get issues:

```php
public function getIssues()
    {
        $data = array();
        $issueDao = \DAORegistry::getDAO('IssueDAO'); /* @var $issueDao \IssueDAO */
        $query = $issueDao->retrieve(
            "SELECT issue_id as id, volume, year, number
            FROM issues
            WHERE journal_id = ".$this->contextId."
            AND published = 1
            ORDER BY date_published DESC
            LIMIT 3"
            );

        while (!$query->EOF) {
            $row = $query->getRowAssoc(false);
            $data[] = [
                'id' => $row['id'],
                'volume' => $row['volume'],
                'year' => $row['year'],
                'number' => $row['number'],
            ];
            $query->MoveNext();
        }

        $query->Close();
        return $data;
    }
```
---
### Journal-id

```php
$context = $params["context"];
$dirFiles = $params['temporaryFullFilePath'];
if(!$context) {
    throw new \Exception("Revista no encontrada");
}
$this->contextId = $context->getId();

$text = "Datos de la revista\n";
$text .= "Nombre: " . $context->getSetting('name', \AppLocale::getLocale()) . "\n";
$text .= "ISSN: " . $context->getSetting('printIssn') . "\n";
$text .= "ISSN electrónico: " . $context->getSetting('onlineIssn') . "\n";
$text .= "Entidad: " . $context->getSetting('publisherInstitution');
```
---
### Submissions info

```php
$context = $params["context"];
$dirFiles = $params['temporaryFullFilePath'];
if(!$context) {
    throw new \Exception("Revista no encontrada");
}
$this->contextId = $context->getId();

try {
    $locale = \AppLocale::getLocale();
    $text = '';

    $authorGuidelines = strip_tags($context->getData('authorGuidelines', $locale));
    if ($authorGuidelines) {
        $text .= __('about.authorGuidelines') . "\n\n" . $authorGuidelines . "\n";
        $text .= "\n*************************\n\n";
    }

    $dataCheckList = $this->getSubmissionChecklist($locale);
    if ($dataCheckList) {
        $text .= __('about.submissionPreparationChecklist') . "\n\n" . $dataCheckList . "\n";
        $text .= "\n*************************\n\n";
    }

    $dataSection = '';
    $sections = \Application::getSectionDAO()->getByContextId($context->getId())->toArray();
    foreach ($sections as $section) {
        $dataSection .= "-" . $section->getLocalizedTitle() . "\n" . strip_tags($section->getLocalizedPolicy()) . "\n\n";
    }

    if ($dataSection) {
        $text .= "Secciones\n\n";
        $text .= $dataSection . "\n";
        $text .= "\n*************************\n\n";
    }

    $copyrightNotice = strip_tags($context->getData('copyrightNotice', $locale));
    if ($copyrightNotice) {
        $text .= __('about.copyrightNotice') . "\n\n" . $copyrightNotice . "\n";
        $text .= "\n*************************\n\n";
    }

    $privacyStatement = strip_tags($context->getData('privacyStatement', $locale));
    if ($privacyStatement) {
        $text .= __('about.privacyStatement') . "\n\n" . $privacyStatement . "\n";
    }

    if(isset($params['exportAll'])) {
        file_put_contents($dirFiles . '/envios.txt', $text);
    } else {
        HTTPUtils::sendStringAsFile($text, "text/plain", "envios.txt");
    }
} catch (\Exception $e) {
    throw new \Exception('Se ha producido un error:' . $e->getMessage());
}

public function getSubmissionChecklist($locale)
{
    $journalDao = \DAORegistry::getDAO('JournalDAO'); /* @var $journalDao \JournalDAO */
    $query = $journalDao->retrieve("
        SELECT setting_value
        FROM journal_settings
        WHERE setting_name='submissionChecklist'
        AND journal_id=".$this->contextId."
        AND locale='".$locale."';
    ");

    $content = '';
    while (!$query->EOF) {
        $row = $query->getRowAssoc(false);
        $fixedValue = $this->fixSerializedString($row['setting_value']);
        $data = unserialize($fixedValue);

        foreach ($data as $value) {
            if(!empty($value['content'])) $content .= '-'.strip_tags($value['content'])."\n\n";
        }

        $query->MoveNext();
    }

    $query->Close();
    return $content;
}
```

---
### URLS

```php
$context = $params["context"];
$request = $params["request"];
$dirFiles = $params['temporaryFullFilePath'];
if(!$context) {
    throw new \Exception("Revista no encontrada");
}
$this->contextId = $context->getId();

$router = $request->getRouter();
$dispatcher = $router->getDispatcher();

$text = "Home\n" . $dispatcher->url($request, ROUTE_PAGE, null, 'index', null, null) . "\n\n";
$text .= "Equipo editorial\n" . $dispatcher->url($request, ROUTE_PAGE, null, 'about', 'editorialTeam') . "\n\n";
$text .= "Envíos\n" . $dispatcher->url($request, ROUTE_PAGE, null, 'about', 'submissions') . "\n\n";
$text .= "Proceso editorial, periodicidad, politica, ética, preservación\n". $dispatcher->url($request, ROUTE_PAGE, null, 'about');

```
---
### Editorial

Get editorial team:
```php
strip_tags($context->getData('editorialTeam', \AppLocale::getLocale()));
```

Editorial Flow Export:
	-
