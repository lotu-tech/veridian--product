# Feature: OAB Process Search Table Replacement

## Summary
Replace the current group visualization for OAB lawyer process searches with a table format to prevent users from hitting entity limits and improve search performance. Users can review ~700 processes in a table and selectively convert specific processes to lawsuit entities on the investigation board.

## User Journeys
1. User searches for processes by OAB number
2. Results display in a table format (similar to existing web search table) showing ~700 processes
3. User applies filters by process parts and date to narrow results
4. User reviews lawyer statistics (main courts, case types like civil) displayed alongside table
5. User clicks on a specific process row to convert it to a lawsuit entity on the investigation board

## Acceptance Criteria
- [ ] OAB searches display results in table format instead of group visualization
- [ ] Table supports filtering by process parts and date
- [ ] Table displays lawyer statistical data (main courts, case nature)
- [ ] Users can click process rows to add them as lawsuit entities to the board
- [ ] Search performance is faster than current group approach
- [ ] Entity consumption is reduced compared to automatic group creation

## Scale & Volume
- ~700 processes per OAB search result
- Table must handle this volume efficiently with filtering/pagination

## Affected Projects
- investigation-front: New table component for OAB process results, replace group visualization
- search-api: Optimize OAB process search endpoint for table format
- investigation-api: Add endpoint for converting selected process to lawsuit entity

## Constraints
- Keep implementation simple - let development team decide specific table columns
- Reuse existing web search table patterns where possible
- Group visualization for OAB processes will be deprecated