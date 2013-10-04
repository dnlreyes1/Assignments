## Homework 6.1: Follow the Arrows!

* Use your upstream remote to pull homework-6.1.md into your workspace on Cloud9 (hint: git pull upstream master).
 
* Find a section in your projects that has some decent looping and branching code: at least five branches that you can diagram.
* Copy-paste your example into homework-6.1.md and attempt to identify the loop conditions with comments e.g. 

```php
while ( $count < $max ) {
// while $count is less than $max

foreach ( $collection as $item ) {
// until there are no more $items in the $collection"
```

* Save your file locally, git add and git commit it (don't forget: -m 'explain why!'), and git push your changes to your Github account.
* **Bonus points:** open a pull request back to the original repo.

(Not a loop:)
public function getChartByIndex($index = null)
    {
        $chartCount = count($this->_chartCollection);
        if ($chartCount == 0) {
            return false;
        }
        if (is_null($index)) {
            $index = --$chartCount;
        }
        if (!isset($this->_chartCollection[$index])) {
            return false;
        }

        return $this->_chartCollection[$index];
    }

(Actual Loop:)
    /**
     * Return an array of the names of charts on this worksheet
     *
     * @return string[] The names of charts
     * @throws PHPExcel_Exception
     */
    public function getChartNames()
    {
        $chartNames = array();
        foreach($this->_chartCollection as $chart) {
            $chartNames[] = $chart->getName();
        }
        return $chartNames;