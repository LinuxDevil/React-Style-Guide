    

```js
// Bad code
<Button 
  onClick={() => { //... }}
/>

// Good code
const onClickHandler = () => {
  // ..
}

<Button
  onClick={onClickHandler}
/>
```

1.  Extract logic outside the HTML
    

```js
// Bad code
return (
  candidates.map((candidate) => {
    <Candidate 
      candidateName={candidate.name}
    />
  })
);

// Good code
// Must be seperated in a file inside the related components directory
const CandidateList = () => candidates.map((candidate) => {
    <Candidate 
      candidateName={candidate.name}
    />
});

return (
  <CandidateList candidates={candidates} />
);
```

1.  Please avoid using nested if’s and magic keys
    
