/**
	 * Update row dimensions when inserting/deleting rows/columns
	 *
	 * @param   PHPExcel_Worksheet  $pSheet             The worksheet that we're editing
	 * @param   string              $pBefore            Insert/Delete before this cell address (e.g. 'A1')
	 * @param   integer             $beforeColumnIndex  Index number of the column we're inserting/deleting before
	 * @param   integer             $pNumCols           Number of columns to insert/delete (negative values indicate deletion)
	 * @param   integer             $beforeRow          Number of the row we're inserting/deleting before
	 * @param   integer             $pNumRows           Number of rows to insert/delete (negative values indicate deletion)
	 */
	protected function _adjustRowDimensions($pSheet, $pBefore, $beforeColumnIndex, $pNumCols, $beforeRow, $pNumRows)
	{
		$aRowDimensions = array_reverse($pSheet->getRowDimensions(), true);
		if (!empty($aRowDimensions)) {
			foreach ($aRowDimensions as $objRowDimension) {
				$newReference = $this->updateCellReference('A' . $objRowDimension->getRowIndex(), $pBefore, $pNumCols, $pNumRows);
				list(, $newReference) = PHPExcel_Cell::coordinateFromString($newReference);
				if ($objRowDimension->getRowIndex() != $newReference) {
					$objRowDimension->setRowIndex($newReference);
				}
			}
			$pSheet->refreshRowDimensions();

			$copyDimension = $pSheet->getRowDimension($beforeRow - 1);
			for ($i = $beforeRow; $i <= $beforeRow - 1 + $pNumRows; ++$i) {
				$newDimension = $pSheet->getRowDimension($i);
				$newDimension->setRowHeight($copyDimension->getRowHeight());
				$newDimension->setVisible($copyDimension->getVisible());
				$newDimension->setOutlineLevel($copyDimension->getOutlineLevel());
				$newDimension->setCollapsed($copyDimension->getCollapsed());
			}
		}
	}
// function _adjustRowDimensions
// invocation for protected function _adjustRowDimensions is in Reference Helper file; line 529
        // Update worksheet: row dimensions
		$this->_adjustRowDimensions($pSheet, $pBefore, $beforeColumnIndex, $pNumCols, $beforeRow, $pNumRows);
// arguements; $pSheet, $pBefore, $beforeColumnIndex, $pNumCols, $beforeRow, $pNumRows
// return value; updated row dimensions

     /**
     * Refresh row dimensions
     *
     * @return PHPExcel_Worksheet
     */

     public function refreshRowDimensions()
     {
         $currentRowDimensions = $this->getRowDimensions();
         $newRowDimensions = array();

         foreach ($currentRowDimensions as $objRowDimension) {
             $newRowDimensions[$objRowDimension->getRowIndex()] = $objRowDimension;
         }

         $this->_rowDimensions = $newRowDimensions;

         return $this;
     }
// function refreshRowDimensions()
// invocation for public function refreshRowDimensions() is in Reference Help file; line 361
        $pSheet->refreshRowDimensions();
// arguements; no arguements
// return value; PHPExcel_Worksheet