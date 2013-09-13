 public function addSheet(PHPExcel_Worksheet $pSheet, $iSheetIndex = NULL)
    {
        if ($this->sheetNameExists($pSheet->getTitle())) {
            throw new PHPExcel_Exception(
            	"Workbook already contains a worksheet named '{$pSheet->getTitle()}'. Rename this worksheet first."
            );
        }

        if($iSheetIndex === NULL) {
            if ($this->_activeSheetIndex < 0) {
                $this->_activeSheetIndex = 0;
            }
            $this->_workSheetCollection[] = $pSheet;
        } else {
            // Insert the sheet at the requested index
            array_splice(
                $this->_workSheetCollection,
                $iSheetIndex,
                0,
                array($pSheet)
                );

            // Adjust active sheet index if necessary
            if ($this->_activeSheetIndex >= $iSheetIndex) {
                ++$this->_activeSheetIndex;
            }
        }
        return $pSheet;
    }

    /**
     * Remove sheet by index
     *
     * @param  int $pIndex Active sheet index
     * @throws PHPExcel_Exception
     */
    public function removeSheetByIndex($pIndex = 0)
    {

        $numSheets = count($this->_workSheetCollection);

        if ($pIndex > $numSheets - 1) {
            throw new PHPExcel_Exception(
            	"You tried to remove a sheet by the out of bounds index: {$pIndex}. The actual number of sheets is {$numSheets}."
            );
        } else {
            array_splice($this->_workSheetCollection, $pIndex, 1);
        }
        // Adjust active sheet index if necessary
        if (($this->_activeSheetIndex >= $pIndex) &&
            ($pIndex > count($this->_workSheetCollection) - 1)) {
            --$this->_activeSheetIndex;
        }

    }

    /**
     * Get sheet by index
     *
     * @param  int $pIndex Sheet index
     * @return PHPExcel_Worksheet
     * @throws PHPExcel_Exception
     */
    public function getSheet($pIndex = 0)
    {

        $numSheets = count($this->_workSheetCollection);

        if ($pIndex > $numSheets - 1) {
            throw new PHPExcel_Exception(
            	"Your requested sheet index: {$pIndex} is out of bounds. The actual number of sheets is {$numSheets}."
           	);
        } else {
            return $this->_workSheetCollection[$pIndex];
        }
    }

    /**
     * Get all sheets
     *
     * @return PHPExcel_Worksheet[]
     */
    public function getAllSheets()
    {
        return $this->_workSheetCollection;
    }

    /**
     * Get sheet by name
     *
     * @param  string $pName Sheet name
     * @return PHPExcel_Worksheet
     */
    public function getSheetByName($pName = '')
    {
        $worksheetCount = count($this->_workSheetCollection);
        for ($i = 0; $i < $worksheetCount; ++$i) {
            if ($this->_workSheetCollection[$i]->getTitle() === $pName) {
                return $this->_workSheetCollection[$i];
            }
        }
