# Token Ejector

If you are developing a smart contract that plans to hold Ditto clones, it is worth considering implementing the token ejector.

```javascript
interface IERC721TokenEjector {

    function onERC721Ejected(
        address operator,
        address to,
        uint256 id,
        bytes calldata data
    ) external returns (bytes4);

}
```
Because a clone can be force transferred to a new owner via the Ditto auction mechanism, the Ditto contract will try to call the `onERC721Ejected` function on the previous owner.

```javascript
if (from.code.length != 0) {
    try IERC721TokenEjector(from).onERC721Ejected{gas: 30000}(address(this), to, id, "") {} // EXTERNAL CALL
    catch {}
}
```

This will allow any smart contract that implements the token ejector a chance to do their own accounting or enact any necessary actions when the clone is force transferred from them.

The function call is given up to a `30,000` gas stipend to accomplish its goals. This should be more than enough gas to get anything reasonable done.

If your contract does not implement `onERC721Ejected` the forced transfer will proceed as intended in any case.

It is highly recommended when implementing `onERC721Ejected` to check that `msg.sender` and/or `operator` are coming from the expected address. Not properly checking these could result in a potential exploit or unintended behaviour.
